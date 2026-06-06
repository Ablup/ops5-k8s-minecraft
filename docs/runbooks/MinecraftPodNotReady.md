### **Runbook 2: MinecraftPodNotReady**

* **Trigger Condition:** kube\_pod\_status\_ready{namespace="default", condition="true"} \== 0 for 1 minute.  
* **Justification:** This detects when the Minecraft server is "Up" (running) but not "Ready" (not accepting connections). This usually means the Java application is hung, the world is still loading, or the readiness probe is misconfigured.  
* **First-Response Steps:**  
  1. **Check status:** Run kubectl get pods. If it is 0/2, the readiness probe is failing.  
  2. **Check Probes:** Run kubectl describe pod \<pod-name\> to see which specific probe (liveness or readiness) is failing.  
  3. **Check Server Logs:** Run kubectl logs \<pod-name\> \-c minecraft to see if the Java engine reports a java.lang.OutOfMemoryError or world corruption.  
  4. **Wait:** If the logs show "Preparing spawn area," the server is just slow; wait for the readiness probe to time out naturally.
