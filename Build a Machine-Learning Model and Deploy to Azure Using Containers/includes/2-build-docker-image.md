# Build a Docker image and deploy to Azure

TODO: Add introduction.

## Deploy an Azure Container Registry

In this exercise, you will create your own Azure Container Registry, which acts as a repository for Docker images in much the same way that [Docker Hub](https://hub.docker.com/) does. The difference is that when you run container images in Azure, the containers start faster if the images that they load are hosted in Azure, too — especially if both reside in the same data center

1. Open your browser and navigate to https://shell.azure.com to launch the Azure Cloud Shell. If you are asked to log in, do so using your Microsoft account.

1. Type the following command to list the Azure subscriptions associated with your Microsoft account:

	```bash
	az account list
	``` 

	The default subscription — the one used to create resources created with the Cloud Shell — is marked `isDefault=true`. If that's the subscription you wish to use, or if it's the only subscription in the list, you're done. Otherwise, use the following command to designate one of the other subscriptions as the default, replacing SUBSCRIPTION_ID with the ID of that subscription: 

	```bash
	az account set -s SUBSCRIPTION_ID
	```

1. Use the following command to create a resource group named "azure-ml-rg" to hold all the Azure resources you create in this module:

	```bash
	az group create  --name azure-ml-rg --location northcentralus
	```

	Resource groups are an incredibly important feature of Azure. They act as containers for other Azure resources and serve to group those resources together so you can view billing information for them as a group, apply security rules as a group, and even delete them as a group. *Every* Azure resource that you create must be part of a resource group.

2. Execute the following command in the Cloud Shell to deploy an Azure Container Registry, replacing TK with TK, TK with TK, and TK with TK:

	```bash

	```

1. TODO: Get an access key for the container registry.

TODO: Add closing.

## Write an app and a Dockerfile

In this exercise, you will create a file named **app.py** containing the Python code that runs inside the container, and a file named **Dockerfile** containing Docker build instructions. You can use any text editor you'd like, but we recommend using [Visual Studio Code](https://code.visualstudio.com/) — Microsoft's free, lightweight source-code editor for Windows, macOS, and Linux that features IntelliSense, integrated Git support, and more

1. Create a file named **app.py** in the same folder in which you stored **sentiment-analysis.pkl** and **vocabulary.pkl** in the previous lesson and insert the following Python code. Then save the file.

	```python
	import pickle, re
	from sklearn.feature_extraction.text import CountVectorizer
	from flask import Flask, request, jsonify
	
	app = Flask(__name__)
	model = pickle.load(open('sentiment-analysis.pkl', 'rb'))
	vocab = pickle.load(open('vocabulary.pkl', 'rb'))
	vectorizer = CountVectorizer(ngram_range=(1, 2),
	    stop_words=['the', 'and', 'am', 'are'],
	    vocabulary=vocab))
	
	@app.route('/predict', methods=['POST'])
	def predict():
	    json = request.get_json()
	    return str(analyze_text(json['text']))
	
	def analyze_text(text, model, vectorizer):
	    text = re.sub("[.;:!\'?,\"()\[\]]", '', text.lower())
	    text = re.sub("(<br\s*/><br\s*/>)|(\-)|(\/)", ' ', text)
	    return model.predict_proba(vectorizer.transform([text]))[0][1]

	if __name__ == '__main__':
	    app.run(debug=True, port=8008, host='0.0.0.0')
	```

	This file contains a Python script that uses [Flask](http://flask.pocoo.org/) to expose a REST method named ```predict``` that clients can call to analyze a string for sentiment. Calls to `pickle.load()` load the serialized machine-learning model and the vocabulary with which it was trained.

1. Create a file named **Dockerfile** (no file-name extension) in the same folder and insert the following commands:

	```docker
	FROM python:3.6.7-stretch
	RUN pip install flask numpy scipy pandas scikit-learn==0.19.1 && \
	    mkdir /app
	COPY app.py /app
	COPY sentiment-analysis.pkl /app
	COPY vocabulary.pkl /app
	WORKDIR /app
	EXPOSE 8008
	ENTRYPOINT ["python"]
	CMD ["app.py"]
	```

	This file contain instructions for building a Docker image that includes **app.py**, **sentiment-analysis.pkl**, and **vocabulary.pkl**, and that launches **app.py** when the container starts and listens for incoming requests on port 8008.


TODO: Add closing.

## Build a Docker image

You can use the [Docker CLI](https://docs.docker.com/engine/reference/commandline/cli/) as well as a number of third-party tools to build Docker images and push them to an Azure Container Registry. However, most of these tools require you to install software on your PC or laptop. A faster and more seamless way to build Docker images and push them to an Azure Container Registry is to use the Azure Cloud Shell, which lets you build Docker images using [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) commands in the browser. Commands such as [`az acr build`](https://docs.microsoft.com/cli/azure/acr?view=azure-cli-latest#az-acr-build) let you build a Docker image from a [Dockerfile](https://docs.docker.com/engine/reference/builder/) and push it to an Azure Container Registry in one simple step, and without having to externally authenticate with the container registry.

In this exercise, you will use the Cloud Shell to build a Docker image from a Dockerfile and push it to the container registry you deployed in the previous exercise.

1. Return to the Cloud Shell and click the **Upload/Download** button at the top of the page. Then select **Upload** from the menu.

	![Uploading files to the Cloud Shell](media/upload-files.png)

	_Uploading files to the Cloud Shell_

1. Browse to the folder containing **app.py**, **Dockerfile**, and the two **.pkl** files. Then upload these files to the Cloud Shell. Note that some browsers might require you to upload one file at a time.

1. In the Cloud Shell, use the following command to build a Docker image using the assets uploaded to the Cloud Shell and push the image to the container registry you deployed in the previous exercise, replacing REGISTRY_NAME with the name assigned to the container registry (for example, "containerslab"). BE SURE TO INCLUDE THE PERIOD AT THE END OF THE COMMAND:

	```bash
	az acr build --registry REGISTRY_NAME --image text-analytics-server .
	```

	Note that you can press **Shift+Ins** to paste the contents of the clipboard into the Cloud Shell if you are running Windows, or **Cmd+V** if you are on a Mac.
 
1. Wait for the build to complete and confirm that it completed successfully. The build will probably take a minute or two.

If you would like to confirm that the Docker image was built and pushed to the container registry, go to the container registry in the [Azure Portal](https://portal.azure.com) and click **Repositories** in the menu on the left side of the blade. You will see a list of images present in the registry, and the list should include the image named "text-analytics-server" that you just built.