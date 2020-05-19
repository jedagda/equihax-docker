FROM alpine:3.7 AS BUILD
 
COPY ./struts2-showcase-2.3.12.war /
#COPY ./Python-3.7.4.tar.xz /

 
FROM tomcat:alpine
#COPY --from=BUILD /Python-3.7.4.tar.xz /
#RUN cd / && ://www.facebook.com/tar -xf Python-3.7.4.tar.xz && rm Python-3.7.4.tar.xz && cd Python-3.7.4 && make -j 8 
COPY --from=BUILD /struts2-showcase-2.3.12.war /usr/local/tomcat/webapps/super-app.war
ADD  ./equifax-website/ /usr/local/

#RUN cp -r /usr/local/equifax-website/equifaxWebsite /usr/local/tomcat/ &&	cp /usr/local/equifax-website/index.html /user/local/tomcat


ENV PS1="\\u@\h:\\w $ "


