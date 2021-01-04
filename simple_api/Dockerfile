FROM python:2.7-stretch
#Maintainer of image
LABEL maintainer="175777@supinfo.com"
#Install of dependencies
RUN apt-get update -y && apt-get install python-dev python3-dev libsasl2-dev python-dev libldap2-dev libssl-dev -y
RUN pip install flask flask_httpauth flask_simpleldap python-dotenv
#COPY SOURCE CODE IN THE IMAGE
RUN mkdir /data
COPY simple_api/student_age.py /data

RUN mkdir -p /var/www/html
COPY website /var/www/html
#CREATE DATA FOLDER
#VOLUME [ "/data" ]
#EXPOSE PORT
EXPOSE 5000
#RUN CODE
CMD [ "python", "./student_age.py" ]
