FROM python:3.9
WORKDIR /app
ADD . /app
EXPOSE 8081
COPY requirements.txt requirements.txt
RUN pip3 install -r requirements.txt
CMD ["main.py"]
ENTRYPOINT ["python"]