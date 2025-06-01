---
layout: post
title:  "Infrastructure Magic : LLMs and Vertex AI"
date:   2025-04-14 19:39:46 -0500
categories: gcp cloud ai vertex infrastructure
---

Infrastructure Magic : LLMs and Vertex AI

Hey AI enthusiasts! Ever felt the thrill of building something truly groundbreaking? That's exactly how I feel diving into the world of Large Language Models (LLMs) on Google Cloud's Vertex AI. Its like wielding a superpower, but instead of capes, we're using gcloud and Terraform! Let's embark on this journey together, exploring the ins and outs of setting up the perfect AI infrastructure.
Infrastructure with Terraform

First things first, let's lay a solid foundation infrastructure as Code (IaC) is our best friend, and Terraform makes it a breeze. Here's a snippet to get us stared, creating a Vertex AI Workbench instance:
TERRAFORM: Vertex AI Workbench Instance

resource "google_vertex_ai_workbench_instance" "llm_workbench" {
  provider = google-beta
  name =  "llm-workbench-instance"
  location = "us-central"
  machine_type = "n1-standard-4"
  owner = "email@example.com"


  vm_image{
    image_family = "tf-latest-cpu"
  }

  labels = {
    env = "development"
  }
}

This code spins up a Vertex AI Workbench instance, perfect for experimenting with LLMs. Notice how were using google-beta to access the latest features!
Deploying an LLM

Now, lets get our hand dirty with gcloud. We'll deploy a pre-trained LLM using the Vertex AI API.
BASH

              
gcloud ai endpoints create llm-endpoint\
--region=us-central1

gcloud ai models deploy \
--model=text-bison@001 \
--endpoint=llm-endpoint \
--region=central1 \
--machine-type=n1-standard-4 \
--traffic-split=deployed-model=100

These commands create an endpoint and deploy the test-bison@001 model. It's amazing how quickly we go from zero to a fully deployed LLM!
Python Magic

Let's use the Vertex AI Python SDK to interact with our deployed model:
PYTHON

              
from google.cloud import aiplatform

aiplatform.init(project="my-project_id", location="us-central1")

endpoint = aiplatform.Endpoint("llm-endpoint")

response = endpoint.predict(
  instance[{"content": "Tell me a short story about a coding bird."}]
)

print(response.predictions[0]["content"])

BOOM! We're talking to an LLM. The possibilities are endless!
Keeping Things Robust

Building a robust AI system isn't just about deploying. It's about scaling and monitoring. Vertex AI makes this easy. We can use Terraform to configure autoscaling:
TERRAFORM : Autoscaling

resource "google_vertex_ai_endpoint" "llm_endpoint" {
  provider = google-beta
  name =  "llm-endpoint"
  location = "us-central"

  deployed_models {
    model = "text-bison@001"
    traffic_split {
      "0" = 100
    }
    autoscaling_config {
      min_replica_count = 1
      max_replica_count = 5 
    }
  }
}

And for monitoring, Vertex AI provides build in tools to track performance and usage.
We are in the Future Now!

Working with LLMs on Vertex AI is like unlocking a new dimension of creativity and problem solving. It's more than just tech, it's about building tools that can change the world. If you're a seasoned AI engineer or jet staring, the power of Vertex AI is with reach.

What are you building with LLMs? Share your projects and experiences! Let's learn and grow together.
