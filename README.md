### 📂 Resource Orchestration & Optimization

In a dual-stack environment (BTC + ALGO) on a single NVMe, CPU and I/O contention are inevitable during Initial Block Download (IBD) and Catchpoint Verification. 

#### The Challenge
Bitcoin's IBD consumed 125% CPU and high I/O, stalling Algorand's cryptographic verification of 21.5M accounts (Catchpoint).

#### The Engineering Solution
1. **Dynamic Prioritization:** Implemented `systemd` resource controls.
2. **CPU Management:** Set Bitcoin to `Nice=15` (lower priority) to ensure the Algorand `algod` process has sufficient cycles for hashing and verification.
3. **I/O Scheduling:** Utilized `ionice` (Class 2, Priority 7) for Bitcoin, ensuring Algorand's database commits take precedence on the NVMe bus.
4. **Strategic Suspension:** Temporarily suspended non-critical node sync to allow high-priority "State Jumps" (Fast Catchup) to complete without thermal or I/O throttling.
