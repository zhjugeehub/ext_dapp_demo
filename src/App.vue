<template>
  <button type="button" @click="getAcct">Connect</button>

  <button type="button" @click="getChainId">ChainId</button>

  <button type="button" @click="stake">Stake</button>
</template>

<script setup lang="ts">
import {ref} from "vue";
import { ethers, Contract, parseUnits } from "ethers";
import stakeJson from "./assets/EraOfExplorersStake.json"

const WINDOW: any = window;

const account = ref("");

async function getAcct() {
    try {
        const accounts = await WINDOW.ethereum.request({
            method: "eth_requestAccounts",
            jsonrpc: "2.0",
        });
        if (!accounts || accounts.length == 0) {
            return "";
        }
        console.log("getAcct", accounts);
        account.value = ethers.getAddress(accounts[0]);
    } catch (err: any) {
        console.log("[E]getAcct", err);
        alert("Unlock Wallet or Not Install Wallet");
        return "";
    }
}

async function getChainId() {
  try {
        const chainId: string = await WINDOW.ethereum.request({method: 'eth_chainId'});
        const chainIdVal = parseInt(chainId).toString();
        console.log(chainIdVal);
    } catch (err: any) {
        console.log("[E]getChainId", err);
    }
}

async function stake() {
  const provider = new ethers.BrowserProvider(WINDOW.ethereum);
  const signer = await provider.getSigner();
  const stakeCont = new Contract("0x5728A8b7CF3eA812525baCBF0B37D34DE58C6d6B", stakeJson, signer);
  console.log(parseUnits("1000000", 8));
  const tx = await stakeCont.stake(30, parseUnits("1000000", 8));
  console.log(tx);
}
</script>
