## Election Contract

The `Election` contract is a Solidity smart contract that facilitates a decentralized voting system on the Ethereum blockchain. It allows an owner to set up an election with a list of candidates, authorize voters, and conduct the voting process in a secure and transparent manner.

### Contract Structure

1. **Candidate Struct**: This struct represents a candidate in the election. It contains two properties:
   - `name`: The name of the candidate (string).
   - `voteCount`: The number of votes the candidate has received (uint).

2. **Voter Struct**: This struct represents a voter in the election. It contains three properties:
   - `weight`: The weight of the voter's vote (uint). In this implementation, all voters have a weight of 1.
   - `voted`: A boolean flag indicating whether the voter has already voted or not.
   - `voterId`: The index of the candidate the voter has voted for (uint).

3. **State Variables**:
   - `name`: The name of the election (string).
   - `owner`: The address of the contract owner (address).
   - `maxVoteCount`: The maximum number of votes received by any candidate (uint).
   - `voters`: A mapping of addresses to `Voter` structs, representing all authorized voters.
   - `candidates`: An array of `Candidate` structs, representing all candidates in the election.
   - `electionEnd`: The timestamp (in Unix time) when the election ends (uint).

4. **Events**:
   - `ElectionResult`: This event is emitted when the election results are announced, providing the name and vote count of a candidate.

5. **Constructor**: The constructor function is called when the contract is deployed. It takes three parameters:
   - `_name`: The name of the election (string).
   - `_duration`: The duration of the election in minutes (uint).
   - `_candidates`: An array of candidate names (string array).

   The constructor initializes the contract by setting the owner, calculating the `electionEnd` timestamp based on the provided duration, and creating `Candidate` structs for each candidate name in the provided array.

6. **Functions**:
   - `authorise(address _voter)`: This function can only be called by the contract owner. It authorizes a voter by setting their `weight` to 1 and marking them as not having voted yet.
   - `vote(uint voterId)`: This function allows a voter to cast their vote for a specific candidate. It checks if the election is still ongoing and if the voter has not already voted. It then records the voter's choice and increments the vote count for the selected candidate.
   - `end()`: This function can only be called by the contract owner after the election has ended. It emits an `ElectionResult` event for each candidate, displaying their name and vote count.
   - `winner()`: This function can only be called by the contract owner. It iterates through all candidates and emits an `ElectionResult` event for any candidate(s) with the maximum number of votes.

### Usage

To use this contract, follow these steps:

1. Deploy the `Election` contract to the Ethereum blockchain, providing the election name, duration (in minutes), and an array of candidate names as constructor arguments.
2. Call the `authorise` function to authorize voters, providing their Ethereum addresses.
3. Once the election starts, authorized voters can call the `vote` function, specifying the index of the candidate they wish to vote for.
4. After the election duration has elapsed, the contract owner can call the `end` function to reveal the results, displaying the name and vote count of each candidate.
5. Alternatively, the contract owner can call the `winner` function to display the name and vote count of the candidate(s) with the maximum number of votes.

Note: This implementation assumes that only the contract owner can authorize voters and manage the election process. It also assumes that each voter has equal voting weight (1). Modifications can be made to the contract logic to accommodate different requirements, such as allowing self-registration of voters or implementing a more complex voting weight system.
