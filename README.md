# Subscription-Payment
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Subscription {
    uint256 public monthlyFee = 0.01 ether;
    mapping(address => uint256) public expiry;

    function subscribe() public payable {
        require(msg.value >= monthlyFee, "Insufficient payment");
        expiry[msg.sender] = block.timestamp + 30 days;
    }

    function renew() public payable {
        require(msg.value >= monthlyFee, "Insufficient");
        if (expiry[msg.sender] > block.timestamp) {
            expiry[msg.sender] += 30 days;
        } else {
            expiry[msg.sender] = block.timestamp + 30 days;
        }
    }

    function isActive(address user) public view returns (bool) {
        return expiry[user] > block.timestamp;
    }
}
