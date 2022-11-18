TP 2 Linux


        I / Un premier serveur web 

            1 ) Installation 

                [pierre@web ~]$ sudo dnf install httpd

                [pierre@web ~]$ sudo vim /etc/httpd/conf/httpd.conf
                Avec la commande :g/^ *#.*/d

                
                [pierre@web ~]$ sudo systemctl start httpd
                
                [pierre@web ~]$ sudo systemctl enable httpd
                Created symlink /etc/systemd/system/multi-user.target.wants/httpd.service → /usr/lib/systemd/system/httpd.service.

                [pierre@web ~]$ sudo ss -alnpt
                State     Recv-Q    Send-Q       Local Address:Port        Peer Address:Port    Process                             
                LISTEN    0         128                0.0.0.0:22               0.0.0.0:*        users:(("sshd",pid=28571,fd=3))    
                LISTEN    0         128                   [::]:22                  [::]:*        users:(("sshd",pid=28571,fd=4))    
                LISTEN    0         511                      *:80                     *:*        users:(("httpd",pid=32356,fd=4),("httpd",pid=32355,fd=4),("httpd",pid=32354,fd=4),("httpd",pid=32352,fd=4))

                [pierre@web ~]$ sudo firewall-cmd --add-port=80/tcp
                success

                [pierre@web ~]$ sudo systemctl is-active httpd
                active

                [pierre@web ~]$ sudo systemctl is-enabled httpd
                enabled

                [pierre@web ~]$ curl localhost
                <!doctype html>
                <html>
                <head>
                    <meta charset='utf-8'>
                    <meta name='viewport' content='width=device-width, initial-scale=1'>
                    <title>HTTP Server Test Page powered by: Rocky Linux</title>
                    <style type="text/css">
                    /*<![CDATA[*/
                    
                    html {
                        height: 100%;
                        width: 100%;
                    }  
                        body {
                background: rgb(20,72,50);
                background: -moz-linear-gradient(180deg, rgba(23,43,70,1) 30%, rgba(0,0,0,1) 90%)  ;
                background: -webkit-linear-gradient(180deg, rgba(23,43,70,1) 30%, rgba(0,0,0,1) 90%) ;
                background: linear-gradient(180deg, rgba(23,43,70,1) 30%, rgba(0,0,0,1) 90%);
                background-repeat: no-repeat;
                background-attachment: fixed;
                filter: progid:DXImageTransform.Microsoft.gradient(startColorstr="#3c6eb4",endColorstr="#3c95b4",GradientType=1); 
                        color: white;
                        font-size: 0.9em;
                        font-weight: 400;
                        font-family: 'Montserrat', sans-serif;
                        margin: 0;
                        padding: 10em 6em 10em 6em;
                        box-sizing: border-box;      
                        
                    }

                
                h1 {
                    text-align: center;
                    margin: 0;
                    padding: 0.6em 2em 0.4em;
                    color: #fff;
                    font-weight: bold;
                    font-family: 'Montserrat', sans-serif;
                    font-size: 2em;
                }
                h1 strong {
                    font-weight: bolder;
                    font-family: 'Montserrat', sans-serif;
                }
                h2 {
                    font-size: 1.5em;
                    font-weight:bold;
                }
                
                .title {
                    border: 1px solid black;
                    font-weight: bold;
                    position: relative;
                    float: right;
                    width: 150px;
                    text-align: center;
                    padding: 10px 0 10px 0;
                    margin-top: 0;
                }
                
                .description {
                    padding: 45px 10px 5px 10px;
                    clear: right;
                    padding: 15px;
                }
                
                .section {
                    padding-left: 3%;
                margin-bottom: 10px;
                }
                
                img {
                
                    padding: 2px;
                    margin: 2px;
                }
                a:hover img {
                    padding: 2px;
                    margin: 2px;
                }

                :link {
                    color: rgb(199, 252, 77);
                    text-shadow:
                }
                :visited {
                    color: rgb(122, 206, 255);
                }
                a:hover {
                    color: rgb(16, 44, 122);
                }
                .row {
                    width: 100%;
                    padding: 0 10px 0 10px;
                }
                
                footer {
                    padding-top: 6em;
                    margin-bottom: 6em;
                    text-align: center;
                    font-size: xx-small;
                    overflow:hidden;
                    clear: both;
                }
                
                .summary {
                    font-size: 140%;
                    text-align: center;
                }

                #rocky-poweredby img {
                    margin-left: -10px;
                }

                #logos img {
                    vertical-align: top;
                }
                
                /* Desktop  View Options */
                
                @media (min-width: 768px)  {
                
                    body {
                    padding: 10em 20% !important;
                    }
                    
                    .col-md-1, .col-md-2, .col-md-3, .col-md-4, .col-md-5, .col-md-6,
                    .col-md-7, .col-md-8, .col-md-9, .col-md-10, .col-md-11, .col-md-12 {
                    float: left;
                    }
                
                    .col-md-1 {
                    width: 8.33%;
                    }
                    .col-md-2 {
                    width: 16.66%;
                    }
                    .col-md-3 {
                    width: 25%;
                    }
                    .col-md-4 {
                    width: 33%;
                    }
                    .col-md-5 {
                    width: 41.66%;
                    }
                    .col-md-6 {
                    border-left:3px ;
                    width: 50%;
                    

                    }
                    .col-md-7 {
                    width: 58.33%;
                    }
                    .col-md-8 {
                    width: 66.66%;
                    }
                    .col-md-9 {
                    width: 74.99%;
                    }
                    .col-md-10 {
                    width: 83.33%;
                    }
                    .col-md-11 {
                    width: 91.66%;
                    }
                    .col-md-12 {
                    width: 100%;
                    }
                }
                
                /* Mobile View Options */
                @media (max-width: 767px) {
                    .col-sm-1, .col-sm-2, .col-sm-3, .col-sm-4, .col-sm-5, .col-sm-6,
                    .col-sm-7, .col-sm-8, .col-sm-9, .col-sm-10, .col-sm-11, .col-sm-12 {
                    float: left;
                    }
                
                    .col-sm-1 {
                    width: 8.33%;
                    }
                    .col-sm-2 {
                    width: 16.66%;
                    }
                    .col-sm-3 {
                    width: 25%;
                    }
                    .col-sm-4 {
                    width: 33%;
                    }
                    .col-sm-5 {
                    width: 41.66%;
                    }
                    .col-sm-6 {
                    width: 50%;
                    }
                    .col-sm-7 {
                    width: 58.33%;
                    }
                    .col-sm-8 {
                    width: 66.66%;
                    }
                    .col-sm-9 {
                    width: 74.99%;
                    }
                    .col-sm-10 {
                    width: 83.33%;
                    }
                    .col-sm-11 {
                    width: 91.66%;
                    }
                    .col-sm-12 {
                    width: 100%;
                    }
                    h1 {
                    padding: 0 !important;
                    }
                }
                        
                
                </style>
                </head>
                <body>
                    <h1>HTTP Server <strong>Test Page</strong></h1>

                    <div class='row'>
                    
                    <div class='col-sm-12 col-md-6 col-md-6 '></div>
                        <p class="summary">This page is used to test the proper operation of
                            an HTTP server after it has been installed on a Rocky Linux system.
                            If you can read this page, it means that the software it working
                            correctly.</p>
                    </div>
                    
                    <div class='col-sm-12 col-md-6 col-md-6 col-md-offset-12'>
                    
                    
                        <div class='section'>
                        <h2>Just visiting?</h2>

                        <p>This website you are visiting is either experiencing problems or
                        could be going through maintenance.</p>

                        <p>If you would like the let the administrators of this website know
                        that you've seen this page instead of the page you've expected, you
                        should send them an email. In general, mail sent to the name
                        "webmaster" and directed to the website's domain should reach the
                        appropriate person.</p>

                        <p>The most common email address to send to is:
                        <strong>"webmaster@example.com"</strong></p>
                    
                        <h2>Note:</h2>
                        <p>The Rocky Linux distribution is a stable and reproduceable platform
                        based on the sources of Red Hat Enterprise Linux (RHEL). With this in
                        mind, please understand that:

                        <ul>
                        <li>Neither the <strong>Rocky Linux Project</strong> nor the
                        <strong>Rocky Enterprise Software Foundation</strong> have anything to
                        do with this website or its content.</li>
                        <li>The Rocky Linux Project nor the <strong>RESF</strong> have
                        "hacked" this webserver: This test page is included with the
                        distribution.</li>
                        </ul>
                        <p>For more information about Rocky Linux, please visit the
                        <a href="https://rockylinux.org/"><strong>Rocky Linux
                        website</strong></a>.
                        </p>
                        </div>
                    </div>
                    <div class='col-sm-12 col-md-6 col-md-6 col-md-offset-12'>
                        <div class='section'>
                        
                        <h2>I am the admin, what do I do?</h2>

                        <p>You may now add content to the webroot directory for your
                        software.</p>

                        <p><strong>For systems using the
                        <a href="https://httpd.apache.org/">Apache Webserver</strong></a>:
                        You can add content to the directory <code>/var/www/html/</code>.
                        Until you do so, people visiting your website will see this page. If
                        you would like this page to not be shown, follow the instructions in:
                        <code>/etc/httpd/conf.d/welcome.conf</code>.</p>

                        <p><strong>For systems using
                        <a href="https://nginx.org">Nginx</strong></a>:
                        You can add your content in a location of your
                        choice and edit the <code>root</code> configuration directive
                        in <code>/etc/nginx/nginx.conf</code>.</p>
                        
                        <div id="logos">
                        <a href="https://rockylinux.org/" id="rocky-poweredby"><img src="icons/poweredby.png" alt="[ Powered by Rocky Linux ]" /></a> <!-- Rocky -->
                        <img src="poweredby.png" /> <!-- webserver -->
                        </div>       
                    </div>
                    </div>
                    
                    <footer class="col-sm-12">
                    <a href="https://apache.org">Apache&trade;</a> is a registered trademark of <a href="https://apache.org">the Apache Software Foundation</a> in the United States and/or other countries.<br />
                    <a href="https://nginx.org">NGINX&trade;</a> is a registered trademark of <a href="https://">F5 Networks, Inc.</a>.
                    </footer>
                    
                </body>
                </html>



            2 ) Avancer vers la maîtrise du service 

                Le service Apache ...
                
                [pierre@web /]$ cat /etc/httpd/conf/httpd.conf 

                [pierre@web /]$ cat /etc/httpd/conf/httpd.conf 

                    User apache
                    Group apache

                [pierre@web /]$ ps -ef

                apache     32353   32352  0 10:35 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
                apache     32354   32352  0 10:35 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
                apache     32355   32352  0 10:35 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
                apache     32356   32352  0 10:35 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND

                [pierre@web /]$ sudo chown apache /usr/share/testpage/index.html 
                [pierre@web /]$ sudo chown apache /usr/share/testpage/

                Changer l'utilisateur utilisé par Apache

                [pierre@web httpd]$ sudo useradd toto -m -s /sbin/nologin
                Creating mailbox file: File exists

                [pierre@web httpd]$ sudo usermod -d /usr/share/httpd/ toto

                [pierre@web httpd]$ sudo vim /etc/httpd/conf/httpd.conf 

                [pierre@web httpd]$ systemctl restart httpd

                [pierre@web httpd]$ ps -ef
                toto       32766   32765  0 11:35 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
                toto       32767   32765  0 11:35 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
                toto       32768   32765  0 11:35 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
                toto       32769   32765  0 11:35 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND

                Faites en sorte que Apache tourne sur un autre port

                [pierre@web httpd]$ sudo vim /etc/httpd/conf/httpd.conf 
                Listen 8888

                [pierre@web httpd]$ sudo firewall-cmd --add-port=8888/tcp --permanent
                success

                [pierre@web httpd]$ sudo firewall-cmd --remove-port=80/tcp --permanent
                success

                [pierre@web httpd]$ sudo systemctl restart httpd

                [pierre@web httpd]$ sudo ss -alnpt
                LISTEN    0         511                      *:8888                   *:*        users:(("httpd",pid=33249,fd=4),("httpd",pid=33248,fd=4),("httpd",pid=33247,fd=4),("httpd",pid=33244,fd=4))

                [pierre@web httpd]$ curl localhost:8888
                <!doctype html>
                <html>
                <head>
                    <meta charset='utf-8'>
                    <meta name='viewport' content='width=device-width, initial-scale=1'>
                    <title>HTTP Server Test Page powered by: Rocky Linux</title>
                    <style type="text/css">
                    /*<![CDATA[*/
                    
                    html {
                        height: 100%;
                        width: 100%;
                    }  
                        body {
                background: rgb(20,72,50);
                background: -moz-linear-gradient(180deg, rgba(23,43,70,1) 30%, rgba(0,0,0,1) 90%)  ;
                background: -webkit-linear-gradient(180deg, rgba(23,43,70,1) 30%, rgba(0,0,0,1) 90%) ;
                background: linear-gradient(180deg, rgba(23,43,70,1) 30%, rgba(0,0,0,1) 90%);
                background-repeat: no-repeat;
                background-attachment: fixed;
                filter: progid:DXImageTransform.Microsoft.gradient(startColorstr="#3c6eb4",endColorstr="#3c95b4",GradientType=1); 
                        color: white;
                        font-size: 0.9em;
                        font-weight: 400;
                        font-family: 'Montserrat', sans-serif;
                        margin: 0;
                        padding: 10em 6em 10em 6em;
                        box-sizing: border-box;      
                        
                    }

                
                h1 {
                    text-align: center;
                    margin: 0;
                    padding: 0.6em 2em 0.4em;
                    color: #fff;
                    font-weight: bold;
                    font-family: 'Montserrat', sans-serif;
                    font-size: 2em;
                }
                h1 strong {
                    font-weight: bolder;
                    font-family: 'Montserrat', sans-serif;
                }
                h2 {
                    font-size: 1.5em;
                    font-weight:bold;
                }
                
                .title {
                    border: 1px solid black;
                    font-weight: bold;
                    position: relative;
                    float: right;
                    width: 150px;
                    text-align: center;
                    padding: 10px 0 10px 0;
                    margin-top: 0;
                }
                
                .description {
                    padding: 45px 10px 5px 10px;
                    clear: right;
                    padding: 15px;
                }
                
                .section {
                    padding-left: 3%;
                margin-bottom: 10px;
                }
                
                img {
                
                    padding: 2px;
                    margin: 2px;
                }
                a:hover img {
                    padding: 2px;
                    margin: 2px;
                }

                :link {
                    color: rgb(199, 252, 77);
                    text-shadow:
                }
                :visited {
                    color: rgb(122, 206, 255);
                }
                a:hover {
                    color: rgb(16, 44, 122);
                }
                .row {
                    width: 100%;
                    padding: 0 10px 0 10px;
                }
                
                footer {
                    padding-top: 6em;
                    margin-bottom: 6em;
                    text-align: center;
                    font-size: xx-small;
                    overflow:hidden;
                    clear: both;
                }
                
                .summary {
                    font-size: 140%;
                    text-align: center;
                }

                #rocky-poweredby img {
                    margin-left: -10px;
                }

                #logos img {
                    vertical-align: top;
                }
                
                /* Desktop  View Options */
                
                @media (min-width: 768px)  {
                
                    body {
                    padding: 10em 20% !important;
                    }
                    
                    .col-md-1, .col-md-2, .col-md-3, .col-md-4, .col-md-5, .col-md-6,
                    .col-md-7, .col-md-8, .col-md-9, .col-md-10, .col-md-11, .col-md-12 {
                    float: left;
                    }
                
                    .col-md-1 {
                    width: 8.33%;
                    }
                    .col-md-2 {
                    width: 16.66%;
                    }
                    .col-md-3 {
                    width: 25%;
                    }
                    .col-md-4 {
                    width: 33%;
                    }
                    .col-md-5 {
                    width: 41.66%;
                    }
                    .col-md-6 {
                    border-left:3px ;
                    width: 50%;
                    

                    }
                    .col-md-7 {
                    width: 58.33%;
                    }
                    .col-md-8 {
                    width: 66.66%;
                    }
                    .col-md-9 {
                    width: 74.99%;
                    }
                    .col-md-10 {
                    width: 83.33%;
                    }
                    .col-md-11 {
                    width: 91.66%;
                    }
                    .col-md-12 {
                    width: 100%;
                    }
                }
                
                /* Mobile View Options */
                @media (max-width: 767px) {
                    .col-sm-1, .col-sm-2, .col-sm-3, .col-sm-4, .col-sm-5, .col-sm-6,
                    .col-sm-7, .col-sm-8, .col-sm-9, .col-sm-10, .col-sm-11, .col-sm-12 {
                    float: left;
                    }
                
                    .col-sm-1 {
                    width: 8.33%;
                    }
                    .col-sm-2 {
                    width: 16.66%;
                    }
                    .col-sm-3 {
                    width: 25%;
                    }
                    .col-sm-4 {
                    width: 33%;
                    }
                    .col-sm-5 {
                    width: 41.66%;
                    }
                    .col-sm-6 {
                    width: 50%;
                    }
                    .col-sm-7 {
                    width: 58.33%;
                    }
                    .col-sm-8 {
                    width: 66.66%;
                    }
                    .col-sm-9 {
                    width: 74.99%;
                    }
                    .col-sm-10 {
                    width: 83.33%;
                    }
                    .col-sm-11 {
                    width: 91.66%;
                    }
                    .col-sm-12 {
                    width: 100%;
                    }
                    h1 {
                    padding: 0 !important;
                    }
                }
                        
                
                </style>
                </head>
                <body>
                    <h1>HTTP Server <strong>Test Page</strong></h1>

                    <div class='row'>
                    
                    <div class='col-sm-12 col-md-6 col-md-6 '></div>
                        <p class="summary">This page is used to test the proper operation of
                            an HTTP server after it has been installed on a Rocky Linux system.
                            If you can read this page, it means that the software it working
                            correctly.</p>
                    </div>
                    
                    <div class='col-sm-12 col-md-6 col-md-6 col-md-offset-12'>
                    
                    
                        <div class='section'>
                        <h2>Just visiting?</h2>

                        <p>This website you are visiting is either experiencing problems or
                        could be going through maintenance.</p>

                        <p>If you would like the let the administrators of this website know
                        that you've seen this page instead of the page you've expected, you
                        should send them an email. In general, mail sent to the name
                        "webmaster" and directed to the website's domain should reach the
                        appropriate person.</p>

                        <p>The most common email address to send to is:
                        <strong>"webmaster@example.com"</strong></p>
                    
                        <h2>Note:</h2>
                        <p>The Rocky Linux distribution is a stable and reproduceable platform
                        based on the sources of Red Hat Enterprise Linux (RHEL). With this in
                        mind, please understand that:

                        <ul>
                        <li>Neither the <strong>Rocky Linux Project</strong> nor the
                        <strong>Rocky Enterprise Software Foundation</strong> have anything to
                        do with this website or its content.</li>
                        <li>The Rocky Linux Project nor the <strong>RESF</strong> have
                        "hacked" this webserver: This test page is included with the
                        distribution.</li>
                        </ul>
                        <p>For more information about Rocky Linux, please visit the
                        <a href="https://rockylinux.org/"><strong>Rocky Linux
                        website</strong></a>.
                        </p>
                        </div>
                    </div>
                    <div class='col-sm-12 col-md-6 col-md-6 col-md-offset-12'>
                        <div class='section'>
                        
                        <h2>I am the admin, what do I do?</h2>

                        <p>You may now add content to the webroot directory for your
                        software.</p>

                        <p><strong>For systems using the
                        <a href="https://httpd.apache.org/">Apache Webserver</strong></a>:
                        You can add content to the directory <code>/var/www/html/</code>.
                        Until you do so, people visiting your website will see this page. If
                        you would like this page to not be shown, follow the instructions in:
                        <code>/etc/httpd/conf.d/welcome.conf</code>.</p>

                        <p><strong>For systems using
                        <a href="https://nginx.org">Nginx</strong></a>:
                        You can add your content in a location of your
                        choice and edit the <code>root</code> configuration directive
                        in <code>/etc/nginx/nginx.conf</code>.</p>
                        
                        <div id="logos">
                        <a href="https://rockylinux.org/" id="rocky-poweredby"><img src="icons/poweredby.png" alt="[ Powered by Rocky Linux ]" /></a> <!-- Rocky -->
                        <img src="poweredby.png" /> <!-- webserver -->
                        </div>       
                    </div>
                    </div>
                    
                    <footer class="col-sm-12">
                    <a href="https://apache.org">Apache&trade;</a> is a registered trademark of <a href="https://apache.org">the Apache Software Foundation</a> in the United States and/or other countries.<br />
                    <a href="https://nginx.org">NGINX&trade;</a> is a registered trademark of <a href="https://">F5 Networks, Inc.</a>.
                    </footer>
                    
                </body>
                </html>

















                


            

                              
