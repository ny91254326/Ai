# Ai
pragma solidity ^0.8.0;

contract AIVotingAdvisor {
    struct Proposal {
        string name;
        uint voteCount;
    }
    
    address public owner;
    mapping(address => uint) public tokens;
    Proposal[] public proposals;
    mapping(address => bool) public hasVoted;
    uint public rewardPerVote = 10;
    
    modifier onlyOwner() {
        require(msg.sender == owner, "Not the contract owner");
        _;
    }
    
    constructor() {
        owner = msg.sender;
        proposals.push(Proposal({name: "Proposal 1", voteCount: 0}));
        proposals.push(Proposal({name: "Proposal 2", voteCount: 0}));
    }
    
    function vote(uint proposalIndex) public {
        require(proposalIndex < proposals.length, "Invalid proposal index");
        require(!hasVoted[msg.sender], "Already voted");
        
        proposals[proposalIndex].voteCount++;
        hasVoted[msg.sender] = true;
        tokens[msg.sender] += rewardPerVote;
    }
    
    function getProposalCount() public view returns (uint) {
        return proposals.length;
    }
    
    function getProposal(uint index) public view returns (string memory, uint) {
        require(index < proposals.length, "Invalid index");
        return (proposals[index].name, proposals[index].voteCount);
    }
    
    function setReward(uint newReward) public onlyOwner {
        rewardPerVote = newReward;
    }
    
    function getAIVoteRecommendation() public view returns (string memory) {
        uint maxVotes = 0;
        string memory recommendedProposal;
        
        for (uint i = 0; i < proposals.length; i++) {
            if (proposals[i].voteCount > maxVotes) {
                maxVotes = proposals[i].voteCount;
                recommendedProposal = proposals[i].name;
            }
        }
        
        return recommendedProposal;
    }
}
