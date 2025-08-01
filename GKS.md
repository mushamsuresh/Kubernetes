✅ Correct Way (Single Cloud Shell Terminal)
You must run curl in a separate tab/window while keeping kubectl port-forward running.

Step-by-Step
1. Run port-forward and keep it running:
**kubectl port-forward pod/nginx 8080:80**
2. Open a new Cloud Shell tab:
In Cloud Shell, click the + icon at the top right → New Terminal
**curl http://localhost:8080**
This will succeed because the port-forward is active in the other terminal.



