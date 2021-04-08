# go1

    pragma solidity ^0.6.0;
    pragma experimental ABIEncoderV2;
    contract userDID {
    address owner; 
    struct Person{
      bytes32 DID;
      string name;
      uint256 age;
      bool isExists;
    }
    mapping(address=>Person) persons; 
    uint256 autoID;
    
    constructor () public {
       autoID=1;
       owner=msg.sender;
    }
    
    function bindID(string memory _name,string memory _sex,uint256 _age) public {
        require(! persons[msg.sender].isExists,"caller must not exists");
        bytes32 did =keccak256(abi.encode(now,autoID));
        Person memory p= Person (did, _name, _age, true);
        persons[msg.sender]=p;
        autoID ++;
        }
    function verifyPerson(bytes32 _did) public view returns(Person memory){
    Person memory p=persons[msg.sender];
    require(p.DID==_did);
    return p;
    }
    function getDID() public view returns(bytes32){
    return persons[msg.sender].DID;
    }
    }
