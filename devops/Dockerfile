FROM ubuntu:20.04

RUN apt update 

RUN apt install python3 python3-pip git -y

# RUN git clone https://group5-braincells.s3.amazonaws.com/python-web-app.zip && \

# unzip python-web-app.zip && \

# rm python-web-app.zip

WORKDIR /app

COPY ./* /app

RUN pip3 install -r requirements.txt

EXPOSE 8000

ENTRYPOINT ["python3"]

CMD ["manage.py", "runserver", "0.0.0.0:8000"]
