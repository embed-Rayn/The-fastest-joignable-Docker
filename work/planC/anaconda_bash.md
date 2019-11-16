```bash
#!/bin/bash
echo "ID 입력하세요."
read user_id
adduser --disabled-password --gecos '' $user_id
adduser $user_id sudo
echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
echo "PassWord를 입력하세요."
passwd $user_id

chmod a+rwx /home/$user_id/

echo "login을 하여 가상환경을 만들겠습니다."
login $user_id
export PATH=/home/administrator/anaconda3/bin:$PATH

echo "가상환경 이름을 입력하세요. e.g.) keras1_env"
read env_name
echo "가상환경에 사용할 파이썬 버전을 입력하세요. e.g) python 3.7이면 3.7입력"
read py_ver
conda create -n $env_name pip python=$py_ver

echo "가상환경을 시작합니다. 키는 명령어 source activate 가상환경이름, 끄는 명령어"
source activate $env_name

pip3 install 
```