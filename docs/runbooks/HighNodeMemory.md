### **Runbook 3: HighNodeMemory**

* **Trigger Condition:** 100 \- (node\_memory\_MemAvailable\_bytes / node\_memory\_MemTotal\_bytes \* 100\) \> 85 for 2 minutes.  
* **Justification:** Minecraft is extremely memory-hungry. When the node approaches 85-90% utilization, the kernel will start killing processes (OOMKiller) to save the system, which will inevitably kill your Minecraft server first.  
* **First-Response Steps:**  
  1. **Verify saturation:** Run kubectl top nodes to confirm which node is at capacity.  
  2. **Find the offender:** Run kubectl top pods \--all-namespaces \--sort-by=memory to see if another system pod has leaked memory.  
  3. **Action:** If Minecraft is the primary consumer, you may need to reduce your MEMORY environment variable in your minecraft-deployment.yaml or resize the EC2 instance to a larger tier.
