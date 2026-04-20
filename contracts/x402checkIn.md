// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract X402CheckIn {
    mapping(address => uint256) public lastCheckIn;
    mapping(address => uint256) public totalCheckIns;

    event CheckedIn(address indexed user, uint256 timestamp);

    function checkIn() external {
        require(
            block.timestamp > lastCheckIn[msg.sender] + 1 hours,
            "Cooldown active"
        );

        lastCheckIn[msg.sender] = block.timestamp;
        totalCheckIns[msg.sender] += 1;

        emit CheckedIn(msg.sender, block.timestamp);
    }

    function getStats(address user) external view returns (uint256 last, uint256 total) {
        return (lastCheckIn[user], totalCheckIns[user]);
    }
}
