<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
  </div>
</template>

<script setup>
import { ref,onMounted } from "vue";
import Web3 from "web3";
import mtContract from '../contracts/MyTokenDemo.json'

//1.web3js
const web3 = new Web3(
  Web3.givenProvider || 'wss://sepolia.infura.io/ws/v3/018c25a8b5ce4f23810c49f60478478f');

console.log('web3', Web3.version);

let contract = new web3.eth.Contract(mtContract.abi, '0x6e401C817Df452C96f384Cae6A096F622b38C989');
console.log(contract);

const getCoinInfo = async()=>{
  try {
    // 请求用户授权调用metamask
    await window.ethereum.enable();
    const account = await window.ethereum.request({
      method: "eth_requestAccounts",
    });
    console.log('account', account);
  } catch (error) {
    // 如果用户拒绝授权
    console.error("User denied account access");
  }
  const blance = await contract.methods.balanceOf('0x6e401C817Df452C96f384Cae6A096F622b38C989').call();
  console.log('blance', blance);
  
}




const msg = ref("hello1");

onMounted(()=>{
  getCoinInfo();
});


</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
