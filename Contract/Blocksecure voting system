// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

/**
 * @title BlockSecure Voting System
 * @dev A decentralized voting system that ensures transparency, security, and immutability.
 */

contract BlockSecureVoting {
    address public admin;
    bool public votingStarted;
    bool public votingEnded;

    struct Candidate {
        string name;
        uint voteCount;
    }

    mapping(address => bool) public hasVoted;
    Candidate[] public candidates;

    constructor() {
        admin = msg.sender;
    }

    modifier onlyAdmin() {
        require(msg.sender == admin, "Only admin can perform this action");
        _;
    }

    modifier duringVoting() {
        require(votingStarted && !votingEnded, "Voting not active");
        _;
    }

    modifier beforeVoting() {
        require(!votingStarted, "Voting already started");
        _;
    }

    // 🔹 Input Function 1: Add a candidate (before voting starts)
    function addCandidate(string memory candidateName) public onlyAdmin beforeVoting {
        candidates.push(Candidate(candidateName, 0));
    }

    // 🔹 Core Function 1: Start the voting
    function startVoting() public onlyAdmin beforeVoting {
        require(candidates.length > 0, "Add at least one candidate before starting");
        votingStarted = true;
    }

    // 🔹 Core Function 2: Cast a vote
    function vote(uint candidateIndex) public duringVoting {
        require(!hasVoted[msg.sender], "You have already voted");
        require(candidateIndex < candidates.length, "Invalid candidate index");

        hasVoted[msg.sender] = true;
        candidates[candidateIndex].voteCount++;
    }

    // 🔹 Core Function 3: End voting
    function endVoting() public onlyAdmin {
        require(votingStarted, "Voting not started yet");
        require(!votingEnded, "Voting already ended");
        votingEnded = true;
    }

    // View function to get the winner
    function getWinner() public view returns (string memory winnerName, uint winnerVotes) {
        require(votingEnded, "Voting not ended yet");
        uint maxVotes = 0;
        uint winnerIndex = 0;
        for (uint i = 0; i < candidates.length; i++) {
            if (candidates[i].voteCount > maxVotes) {
                maxVotes = candidates[i].voteCount;
                winnerIndex = i;
            }
        }
        winnerName = candidates[winnerIndex].name;
        winnerVotes = candidates[winnerIndex].voteCount;
    }

    // View all candidates
    function getCandidates() public view returns (Candidate[] memory) {
        return candidates;
    }
}

