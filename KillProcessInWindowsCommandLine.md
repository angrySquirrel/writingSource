
http://stackoverflow.com/questions/6204003/kill-a-process-by-looking-up-the-port-being-used-by-it-from-a-bat

netstat -o -n -a | findstr 0.0:8080

TCP 0.0.0.0:8080 0.0.0.0:0 LISTENING 9400

taskkill /F /PID 9400
SUCCESS: The process with PID 9400 has been terminated.
