### **Runbook 1: MinecraftPodCrashLooping**

* **Trigger Condition:** increase(kube\_pod\_container\_status\_restarts\_total{namespace="default", pod=\~"minecraft-.\*"}\[5m\]) \> 3 (More than 3 restarts in 5 minutes).  
* **Justification:** A single restart can happen during a patch or a temporary network failure. Multiple restarts in a short window indicate a systemic failure (like an image crash loop).  
* **First-Response Steps:**  
  1. **Identify the pod:** Run kubectl get pods \-l app=minecraft.  
  2. **Check for exit reasons:** Run kubectl describe pod \<pod-name\>. Look specifically at the **Events** section and the **Last State** of the container.  
  3. **Inspect logs:** Run kubectl logs \<pod-name\> \-c mc-monitor \--previous. This will show the error that caused the crash.  
  4. **Rollback:** If this was caused by a new configuration or image, revert immediately: kubectl rollout undo deployment/minecraft.
