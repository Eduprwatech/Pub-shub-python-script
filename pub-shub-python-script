Pub sub code


sudo apt-get update

#Install Python

sudo apt-get install python3-pip
#Installing PubSUB
sudo apt install python3.11-venv
python3 -m venv myenv
source myenv/bin/activate
pip install google-cloud-pubsub




Publisher python code


import asyncio
import time
from google.cloud import pubsub_v1

# Replace with your Google Pub/Sub topic path
project_id = "YOUR_PROJECT_ID"
topic_id = "YOUR_TOPIC_ID"

topic_path = f"projects/project_id/topics/topic_id"

# Initialize the Pub/Sub client
publisher = pubsub_v1.PublisherClient()

async def publish_messages():
    futures = []
    
    for n in range(1, 10):
        data_str = f"Message number {n}"
        # Data must be a bytestring
        data = data_str.encode("utf-8")
        print(data)
        
        # Publish the message and collect the future
        future = publisher.publish(topic_path, data)
        futures.append(future)
        
        # Optionally sleep between messages
        time.sleep(1)
    
    # Wait for all futures to complete
    for future in futures:
        print(await future.result())
    
    print(f"Published messages to {topic_path}.")

# Run the asyncio event loop
asyncio.run(publish_messages())






Subscriber python code


from concurrent.futures import TimeoutError
from google.cloud import pubsub_v1

# Config project and subscription details
project_id = ""YOUR_PROJECT_ID""
subscription_id = "YOUR_TOPIC_ID"

# Number of seconds the subscriber should listen for messages
timeout = 5.0

# Initialize the SubscriberClient
subscriber = pubsub_v1.SubscriberClient()

# The subscription_path method creates a fully qualified identifier
subscription_path = subscriber.subscription_path(project_id, subscription_id)

# Define the callback function
def callback(message: pubsub_v1.subscriber.message.Message) -> None:
    """Print each message received."""
    print(f"Received message: {message.data.decode('utf-8')}")
    message.ack()

# Listen for messages
streaming_pull_future = subscriber.subscribe(subscription_path, callback=callback)
print(f"Listening for messages on {subscription_path}...\n")

try:
    # Wait for messages to arrive (timeout in seconds)
    streaming_pull_future.result(timeout=timeout)
except TimeoutError:
    print("Listening timed out.")
    streaming_pull_future.cancel()  # Trigger the shutdown.
    streaming_pull_future.result()  # Block until the shutdown is complete.
finally:
    subscriber.close()
    print("Subscriber client closed.")


=======================


python3 publisher.py
python3 subscriber.py



  








