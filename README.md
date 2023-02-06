# stock-predictor5

Create the EC2 instance in the AWS management tool.

Use stock-predictor-fastapi for the Key pair name when generating the pem file.

In Step 1-7 Network Setting: click Edit and make sure you have two (or three) security group rules. One has the type SSH with port 22, and another TCP port for the API, e.g., 8000, edit the source.

Once you launch the instance, it should take ~30 seconds to start. Click into your instance. Refresh to see the Connect button is no longer grayed out, and click Connect. In the Connect to instance tab, find SSH client. Move the stock-predicr-fastapi.pem file you downloaded earlier to your local repo. Follow the instruction, change the permissions:

chmod 400 stock-predictor-fastapi.pem

Access the EC2 Instance using ssh. In the same Connect to instance tab, you can find the command to access the instance via ssh, something like:

ssh -i "stock-predictor-fastapi.pem" ec2-user@ec2-35-90-247-255.us-west-2.compute.amazonaws.com

Run the command where stock-predictor-fastapi.pem is located.

Set up environment

sudo yum update -y sudo yum install git -y # install git

install tmux to switch easily between programs in one terminal
sudo yum install tmux

install miniconda and add its path to env
wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda.sh bash ~/miniconda.sh -b -p ~/miniconda echo "PATH=$PATH:$HOME/miniconda/bin" >> ~/.bashrc source ~/.bashrc Clone the repository (use https for convenience) and install all dependencies

git clone https://github.com/[YOUR HANDLER]/stock-predictor.git cd stock-predictor pip install -U pip pip install -r requirements.txt

tmux new -s stock_session

Navigate to the same directory where main.py is (e.g., repo root directory or src) and run the app

uvicorn main:app --reload --workers 1 --host 0.0.0.0 --port 8000

uvicorn main:app --reload --workers 1 --host 0.0.0.0 --port 8000

Now find the Public IPv4 address for your instance, and run the following in another shell on your local machine:

curl
--header "Content-Type: application/json"
--request POST
--data '{"ticker":"MSFT", "days":7}'
http://35.90.247.255:8000/predict
