---
title: "How I Dockerized and Deployed a Jupyter Notebook Server on GCP"
description: "Data science on the go"
date: 2018-09-13T00:14:44-07:00
draft: true
---

I've got a jupyter notebook for running my data science projects at
https://notebooks.ramsgoli.com. Here's how I did it.

## The Motivation
If you're like me, you too have boarded the hype train loaded with aspiring data engineers/scientists.
You've watched some videos and read some books, and have tried playing around with jupyter notebooks,
but often just get frustrated with the tooling required to setup. You want an easy solution for running
a jupyter notebook, without the hastle of managing your python environment, or figuring out what
the heck Anaconda is and how to use it. Oh, and you want it _in the cloud_ because well, why not?<br>
This is where the beauty of running jupyter notebooks inside of a Docker container comes in.

## The Solution
By the end of this short guide, you'll hopefully learn how to:

* Spin up a Docker container for running a minimal Jupyter Notebook
* Password-protect your notebook so that only you can access it when it's running in the cloud
* Use SSL to encrypt your password input

Let's get to it!

## Picking a Cloud Provider
If you want your notebook running in the cloud so that you can access it from _any_ machine you're working on,
you'll have to pick a cloud provider that will spin up a virtual server for you. There are a ton of providers
out there, like AWS, Google Cloud Platform, DigitalOcean, etc. I chose GCP to deploy my notebook on just because
I had a bunch of spare credits, but I would actually recommend using AWS. I personally found it confusing figuring
out how to SSH into my box from my own terminal without using their janky in-browser terminal...
