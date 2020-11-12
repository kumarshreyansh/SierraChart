SierraChart Setup

Instance Ubuntu 18.04
Setup a Security Group to allow ssh connection only from my public IP
Install docker engine on the newly launched EC2 instance as per docker installation documentation
Create a user named as docker user #useradd dockeruser
Switch to docker user#su - dockeruser
Allow dockeruser to run docker as a non-root user#sudo usermod -aG docker dockeruser
Save sc_build_dll.bash, launch.json, tasks.json, c_cpp_properties.json in present working directory as per the attachment


Build a docker image from mentioned Dockerfile

Dockerfile

FROM ubuntu:18.04
#Installing WineHQ packages
RUN mkdir -p /root/SierraChart
RUN cd /root/SierraChart
RUN sudo dpkg --add-architecture i386
RUN wget -nc https://dl.winehq.org/wine-builds/winehq.key
RUN sudo apt-key add winehq.key
RUN sudo add-apt-repository 'deb https://dl.winehq.org/wine-builds/ubuntu/ bionic main'
RUN sudo apt update
RUN sudo apt install --install-recommends winehq-devel

#download sierrachart installer
RUN wget https://www.sierrachart.com/downloads/SierraChartFileDownloader.exe
COPY sc_build_dll.bash /root
COPY launch.json /root/SierraChart
COPY tasks.json /root/SierraChart
COPY c_cpp_properties.json /root/SierraChart
#visual studio setup for c++ development
RUN wget -qO - https://gitlab.com/paulcarroty/vscodium-deb-rpm-repo/raw/master/pub.gpg | sudo apt-key add -
RUN echo 'deb https://gitlab.com/paulcarroty/vscodium-deb-rpm-repo/raw/repos/debs/ vscodium main' | sudo tee --append /etc/apt/sources.list.d/vscodium.list
RUN sudo apt update &amp;&amp; sudo apt install codium
RUN sudo apt install gdb-mingw-w64 g++-mingw-w64-x86-64
RUN wget http://win-builds.org/1.5.0/packages/windows_64/gdb-7.8-1-x86_64-w64-mingw32.txz
