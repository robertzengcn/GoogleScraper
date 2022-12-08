FROM python:3.9
# RUN wget -q https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
# RUN apt-get update&&apt-get install -y ./google-chrome-stable_current_amd64.deb

# Install chrome driver
WORKDIR /app/chromeDriver
# RUN apt-get update
# RUN apt-get install unzip
RUN wget -q https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
RUN apt-get update&&apt-get install -y ./google-chrome-stable_current_amd64.deb

RUN apt-get install unzip
RUN CHROMEVER=$(google-chrome --product-version | grep -o "[^\.]*\.[^\.]*\.[^\.]*") && \
    DRIVERVER=$(curl -s "https://chromedriver.storage.googleapis.com/LATEST_RELEASE_$CHROMEVER") && \
    wget -q --continue "http://chromedriver.storage.googleapis.com/$DRIVERVER/chromedriver_linux64.zip" && \
    unzip chromedriver_linux64.zip
RUN apt-get remove -y unzip    
# RUN /usr/local/bin/python3 -m pip install --upgrade pip
# RUN pip3 install pip -U
#RUN pip3 config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple/
WORKDIR /app/GoogleScraper
RUN pip3 install git+https://github.com/robertzengcn/GoogleScraper.git
RUN sed -i "/chromedriver_path =/c\chromedriver_path = '/app/chromeDriver/chromedriver'" /usr/local/lib/python3.9/site-packages/GoogleScraper/scrape_config.py 
# RUN sed -i "/geckodriver_path =/c\geckodriver_path = '/app/geckoDriver/geckodriver'" /usr/local/lib/python3.9/site-packages/GoogleScraper/scrape_config.py 

RUN apt-get update && apt-get install -y openssh-server
RUN mkdir /var/run/sshd

RUN echo "root:mypassword" | chpasswd
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
#RUN sed -i '/^#/!s/PermitRootLogin .*/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd
RUN service ssh restart


# RUN sed -i "/chromedriver_path =/c\chromedriver_path = '/app/chromeDriver/chromedriver'" /usr/local/lib/python3.9/site-packages/GoogleScraper/scrape_config.py 
EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
