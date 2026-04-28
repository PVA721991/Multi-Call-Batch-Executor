# Multi-Call-Batch-Executor
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract MultiCall {
    error CallFailed(uint256 index);

    function multicall(bytes[] calldata data) public payable returns (bytes[] memory results) {
        results = new bytes[](data.length);
        
        for (uint256 i = 0; i < data.length; i++) {
            (bool success, bytes memory result) = address(this).delegatecall(data[i]);
            if (!success) revert CallFailed(i);
            results[i] = result;
        }
        return results;
    }
}
