1.	Table des matières
1.	TABLE DES MATIÈRES	1
2.	INTRODUCTION	2
2.1.	REMARQUE	2
3.	TÂCHE 1 : INSTALLER LES OUTILS	2
3.1.	VAGRANT	2
3.2.	JMETER	2
3.3.	GIT	2
3.4.	MANIPULATIONS	2
3.5.	RÉPONSES AUX QUESTIONS	2
4.	TÂCHE 2 : STICKY SESSIONS	3
4.1.	RÉPONSES AUX QUESTIONS	3
5.	TÂCHE 3 : DRAIN MODE	4
5.1.	MANIPULATIONS	4
5.2.	RÉPONSES AUX QUESTIONS	4
6.	TÂCHE 4 : ROUND ROBIN IN DEGRADED MODE	5
6.1.	RÉPONSES AUX QUESTIONS	5
7.	TÂCHE 5 : BALANCING STRATEGIES	6
7.1.	RÉPONSES AUX QUESTIONS	6
8.	CONCLUSION	7
9.	TABLE DES ILLUSTRATIONS	8
10.	TABLE DES RÉFÉRENCES	9

 
2.	Introduction
bla
2.1.	Remarque
bla
3.	Tâche 1 : Installer les outils
bla
3.1.	Vagrant
bla
3.2.	JMeter
bla
3.3.	Git
bla
3.4.	Manipulations
bla
3.5.	Réponses aux questions
3.5.1.	Question 1
bla
3.5.2.	Question 2
bla
3.5.3.	Question 3
bla
 
4.	Tâche 2 : Sticky sessions

Sources:
https://www.haproxy.com/fr/blog/load-balancing-affinity-persistence-sticky-sessions-what-you-need-to-know/

4.1.	Réponses aux questions  
4.1.1.	Question 1  
SERVERID:  
In this case, we inject a cookie in the client browser with the ID of the server that
treated him. The next time the client access the site, he will provide this cookie that
will tell the load balancer on which server he should send him.
- On the client first connexion, the HAProxy will send him this header:
"Set-Cookie: SERVERID=s1" if the server chosen is the s1.

- For the following requests, the client will have this header: "Cookie: SERVERID=s1"
in every requests, letting the balancer know on which server he should send him.

NODESESSID:  
In this case, instead of injecting a cookie in the browser, we use the cookie setup
by the application server.
- On the first connexion, the server will set the cookie which will look like this:
"Set-Cookie: NODESESSID=s1~i12KJF23JKJ1EKJ21213KJ"
The cookie has been prefixed by the server cookie value (s1 here). The ~ is used as a separator
between the server information and the cookie value.

- For the following requests, the client will have this header:
"Cookie: NODESESSID=s1~i12KJF23JKJ1EKJ21213KJ"
in every requests, letting the balancer know on which server he should send him.

4.1.2.	Question 2  
To enable the sticky session management, we added the following line to the .cfg under "back_end node":
	cookie SERVERID insert indirect nocache
	server s1 172.17.0.2:3000 check cookie s1
	server s2 172.17.0.3:3000 check cookie s2

The first one tell the HAProxy to setup a cookie called SERVERID if the user don't have one already.
The nocache argument add the "Cache-Control: nocache" to the header since we don't want a personnal
cookie to be stored in any cache.
The next two lines tell the HAProxy to check the value of the cookie and which server to choose given
it's value.

4.1.3.	Question 3  
When we open the url for the first time, the HAProxy inject a cookie in our browser containing the server that received us (TODO: add 2_first_access.png).
From this point, each time we access this page from this browser, we will be directed to the same server. This is the case as long as the cookie is in the browser. (2_multiple_access.png)
If you close the browser and then access the page from a new one, you will no longer have your cookie so the server may change from the last one and the number of views resets. You receive
a new cookie to start the sticky-session again.

4.1.4.	Question 4  
4.1.5.	Question 5  
4.1.6.	Question 6  


 
5.	Tâche 3 : Drain mode
5.1.	Manipulations
bla
5.2.	Réponses aux questions
5.2.1.	Question 1
bla
5.2.2.	Question 2
bla
5.2.3.	Question 3
bla
5.2.4.	Question 4
bla
5.2.5.	Question 5
bla
5.2.6.	Question 6
bla
5.2.7.	Question 7
bla

 
6.	Tâche 4 : Round robin in degraded mode
6.1.	Réponses aux questions
6.1.1.	Question 1
bla
6.1.2.	Question 2
bla
6.1.3.	Question 3
bla
6.1.4.	Question 4
bla
6.1.5.	Question 5
bla
6.1.6.	Question 6
bla




 
7.	Tâche 5 : Balancing strategies
7.1.	Réponses aux questions
bla
7.1.1.	Question 1
bla
7.1.2.	Question 2
bla
7.1.3.	Question 3
bla




 
8.	Conclusion
bla

 
9.	Table des illustrations
Aucune entrée de table d'illustration n'a été trouvée.
 
10.	Table des références
bla
