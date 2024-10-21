
# AWS CloudWatch 🖥️⌚

<img width="960" alt="18 (2)" src="https://github.com/user-attachments/assets/b8af8b0c-adcc-49f7-bc8e-d2bb131f36eb">


## 📚 Project Overview

This project demonstrates how to simulate a CPU spike in an AWS EC2 instance, monitor it using AWS CloudWatch, and set up alarms to trigger notifications using AWS SNS. The goal is to track CPU utilization in real-time and automatically trigger an email alert when CPU utilization exceeds a certain threshold.

In this project, I:

1. Launched an EC2 instance on AWS.
2. Simulated CPU spikes with a Python script.
3. Monitored CPU utilization using AWS CloudWatch.
4. Set up a CloudWatch Alarm to notify via email when CPU utilization exceeds 50% over a 1-minute average.

## 🔧 AWS Services Used

- **Amazon EC2**: A virtual server to run the CPU simulation.
- **AWS CloudWatch**: Used to monitor the CPU utilization.
- **CloudWatch Alarms**: To trigger alerts based on CPU utilization metrics.
- **Amazon SNS (Simple Notification Service)**: Sends an email notification when the CloudWatch alarm is triggered.

## 🚀 Setup Instructions

### 1. Launch an EC2 Instance

1. Go to the **EC2 Dashboard** on AWS.
2. Click on **Launch Instance**.
3. Choose your desired **AMI** (Amazon Machine Image) and **Instance Type** (e.g., `t2.micro` for testing).
4. Configure **Security Groups** to allow SSH (port 22) access.
5. Launch the instance and connect to it via SSH.

### 2. Install Python on EC2

After connecting to your instance via SSH, ensure Python is installed. If not, install it with:

```bash
sudo yum update -y
sudo yum install python3 -y
```

### 3. Upload and Run the CPU Spike Script

1. Use the following Python script to simulate a CPU spike:

```python
import time

def simulate_cpu_spike(duration=30, cpu_percent=80):
    print(f"Simulating CPU spike at {cpu_percent}%...")
    start_time = time.time()

    # Calculate the number of iterations needed to achieve the desired CPU utilization
    target_percent = cpu_percent / 100
    total_iterations = int(target_percent * 5_000_000)  # Adjust the number as needed

    # Perform simple arithmetic operations to spike CPU utilization
    for _ in range(total_iterations):
        result = 0
        for i in range(1, 1001):
            result += i

    # Wait for the rest of the time interval
    elapsed_time = time.time() - start_time
    remaining_time = max(0, duration - elapsed_time)
    time.sleep(remaining_time)

    print("CPU spike simulation completed.")

if __name__ == '__main__':
    # Simulate a CPU spike for 30 seconds with 80% CPU utilization
    simulate_cpu_spike(duration=30, cpu_percent=80)
```

2. Save this script as `cpu_spike.py` on the EC2 instance.
3. Run the script to simulate CPU load:

```bash
python3 cpu_spike.py
```

### 4. Set Up CloudWatch Monitoring

1. Navigate to **CloudWatch** in the AWS Console.
2. Under **Metrics**, select **EC2** and choose your instance's **CPUUtilization** metric.
3. You can view the real-time CPU usage after running the script.

### 5. Set Up SNS for Notifications

Amazon Simple Notification Service (SNS) allows you to send messages to users via email, SMS, or other protocols. Here’s how you can set up SNS to send an email when a CloudWatch Alarm is triggered:

#### Step 1: Create an SNS Topic

1. Go to the **SNS Dashboard** in AWS.
2. Click **Create Topic**.
   - **Type**: Standard
   - **Name**: `cpu-alarm-topic`
3. Click **Create Topic**.

#### Step 2: Subscribe an Email to the SNS Topic

1. In the SNS topic you just created, click on **Create Subscription**.
   - **Protocol**: Email
   - **Endpoint**: Enter your email address.
2. Click **Create Subscription**.

💡 **Note**: You will receive a confirmation email. Make sure to confirm the subscription to start receiving alerts. Also When Directly creating aarms from cloudwatch the topics will be created , to make a subscriber confirm the verification mail sent to the specific user.

### 6. Create a CloudWatch Alarm ⏰

1. In CloudWatch, go to **Alarms** and click **Create Alarm**.
2. Select the **CPUUtilization** metric from your EC2 instance.
3. Set the alarm condition:
   - **Threshold type**: Static
   - **Threshold**: CPU utilization > 50%
   - **Period**: 1 minute
4. Configure actions:
   - **Send notification to**: Select the SNS topic you created (`cpu-alarm-topic`).
5. Review and create the alarm.

## 📊 Monitoring the Spike

Run the Python script, and after about a minute, you’ll see the CPU utilization spike in CloudWatch metrics. When CPU utilization exceeds 50%, you’ll receive an email notification via AWS SNS. Below Are Some Screenshots that are relevant to metrics and alerts

![alarm_metric](https://github.com/user-attachments/assets/2d5bf915-1777-4d79-8520-3289615f13c9)

<br/>

<img width="1423" alt="alert_notification" src="https://github.com/user-attachments/assets/541db17f-a76b-4404-9c95-51997f79a7a9">

<br/>

![spike_metric](https://github.com/user-attachments/assets/6a33e1f4-9758-4d1b-96dd-fb5822c27ec0)

<br/>

<img width="931" alt="subscribe_notification" src="https://github.com/user-attachments/assets/3a3975ed-0574-4f7f-af05-f4b96791943a">

<br/>

<p align="center">
<img width="559" alt="subscription_success" src="https://github.com/user-attachments/assets/33a2e01c-d5fe-48d4-9131-ae7f2a9c9c30">
</p>


## 📝 Resources

- **EC2 Instance**: [AWS EC2](https://aws.amazon.com/ec2/)
- **CloudWatch Alarms**: [AWS CloudWatch Alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html)
- **SNS Notifications**: [AWS SNS](https://aws.amazon.com/sns/)

## 🌟 Conclusion

This project demonstrates a simple yet effective way to monitor system performance using AWS services. By setting up alarms, you can proactively monitor and respond to changes in system performance.

