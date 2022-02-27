---
title: "How to Run CryptoNets in 2022"
date: 2022-02-23T23:53:17+08:00
categories: ["Develop"]
draft: false
---

This is a posty about how to install and run CryptoNets in 2022.
<!--more-->


# Introduction
More and more companies have built applications based on big data techniques in recent years. To have a well-performance model, collecting data has become a must. However, the consciousness of privacy raises in many people’s minds, and they are less willing to give data to companies. As a result, data protection has become a vital technique to balance the dilemma between big data and data privacy.

In 2016, Microsoft Research published CryptoNets [1], the first paper to infer a CNN model in the encrypted domain. The main idea of CryptoNets is to infer encrypted data in the encrypted domain, and the protocol is under a client who has sensible data and a company that has a CNN model. At the beginning of the protocol, a client encrypts his data with his public key and sends it to the company. Next, when the company receives the encrypted image, he infers the image in the encrypted domain and sends the result back to the client. After the client gets the encrypted result, he decrypts the result with his private key, and now he gets the meaning of the result. Because the whole process is in the encrypted domain, the company doesn’t have any information about this image. As a result, CryptoNets can effectively protect data privacy.

Although CryptoNets is good for beginners to know how to infer data in the encrypted domain, after Microsoft Research released related code in 2019, the code doesn’t be updated anymore. In this post, I will tell you how to run CryptoNets in 2022.

# Prerequisites
Before we start, we need the following things:
1. **Visual Studio 2022**

	Microsoft Research wrote CryptoNets in C#, so we need VS to compile the project. The installation instructions are in the next section.
	
2. **SEAL Repo**

	I've updated the setting files for VS 2022, so I recommend you clone the branch ```cryptonets_with_vs_2022``` of [my fork](https://github.com/whcjimmy/SEAL/tree/cryptonets_with_vs_2022).
	
3. **CryptoNets Repo**
	
	You can get the repo from [here](https://github.com/microsoft/CryptoNets).
	
4. **NuGet Package command line tool**
	
	Following the official manual, we need to pack SEAL Library into a NuGet package file. You can get the command-line tool from [here](https://docs.microsoft.com/en-us/nuget/install-nuget-client-tools).
	![image_7](/images/how-to-run-cryptonets-in-2022/2022-02-24-150732.png)


5. **MNIST Dataset**

	You can find the dataset from [here](http://yann.lecun.com/exdb/mnist/). Both training dataset and testing dataset are needed.


# Installation Guides
If eveything is set up, let's get started!
1.	**Install visual studio 2022**

	You need to install VS 2022 with the following requirements:
	
	* Desktop Development with C++
	* .Net Desktop Development
	* C++ Cmake tools for Windows
	* C++ Cmake tools for Linux
	* .Net Framework 4.8
	* .Net Framework 4.8 targeting pack
	* .Net Core 2.1 Runtime (out of support)
![image_1](/images/how-to-run-cryptonets-in-2022/2022-02-24-162559.png)
![image_2](/images/how-to-run-cryptonets-in-2022/2022-02-24-162613.png)
![image_3](/images/how-to-run-cryptonets-in-2022/2022-02-24-162626.png)
![image_4](/images/how-to-run-cryptonets-in-2022/2022-02-24-162646.png)

2.	**Compile SEAL Library** (the folloing steps are under  SEAL Repo)
	1.	Use VS to open  ```./donet/src/SEALNET.csproj``` and accept all review solution actions.
		![image_5](/images/how-to-run-cryptonets-in-2022/2022-02-21-202909.png)
		![image_6](/images/how-to-run-cryptonets-in-2022/2022-02-21-202939.png)
	
	2. Build the following three projects after setting the configuration as ```Release``` and the platform as ```x64```.
		1. ```./native/src/SEAL.vcxproj```
		2. ```./dotnet/native/SEALNetNative.vcxproj``` 
		3. ```./dotnet/src/SEALNet.csproj```
	![image_7](/images/how-to-run-cryptonets-in-2022/2022-02-24-111353.png)
	![image_7](/images/how-to-run-cryptonets-in-2022/2022-02-21-203125.png)
	![image_7](/images/how-to-run-cryptonets-in-2022/2022-02-24-111444.png)

	3. Put NuGet command-line tool into ``` ./dotnet/nuget```  folder.
		![image_8](/images/how-to-run-cryptonets-in-2022/2022-02-24-111723.png)

	4. Open ```./dotnet/nuget``` with CMD and enter the following command to build nuget packages. The SEAL nupkg file is generated under ```./dotnet/nuget/Release```.

		```nuget pack SEALNet.nuspec -properties Configuration=Release -Verbosity detailed -OutputDir Release```
	     ![image_9](/images/how-to-run-cryptonets-in-2022/2022-02-24-111619.png)
		 ![image_9](/images/how-to-run-cryptonets-in-2022/2022-02-24-111704.png)

	5. Copy the SEAL nupkg file to the root directory of CryptoNets Repo.
	![image_9](/images/how-to-run-cryptonets-in-2022/2022-02-24-111814.png)


3. **Compile CryptoNets Project** (the folloing steps are under CryptoNets Repo)
   1. Use VS to open  ```./CryptoNets/CryptoNets.sln``` and updfatre the .Net Framework version.
	![image_10](/images/how-to-run-cryptonets-in-2022/2022-02-21-203947.png)
	![image_11](/images/how-to-run-cryptonets-in-2022/2022-02-21-204004.png)
	
   2. Follow  ```Tool -> Nuget Package Manager -> Manage NuGet Packages for Solution```  to import SEAL nupkg file.

		If you cannot find the SEAL nupgk file, you may need to restart visual studio.
		![image_12](/images/how-to-run-cryptonets-in-2022/2022-02-21-205028.png)
	
   3. Build DataPreprocess and CryptoNets projects after set the configuration as ```Debug``` and the platform as ```x64```, .
	![image_13](/images/how-to-run-cryptonets-in-2022/2022-02-21-205224.png)
	![image_14](/images/how-to-run-cryptonets-in-2022/2022-02-21-205241.png)
	![image_15](/images/how-to-run-cryptonets-in-2022/2022-02-21-204047.png)
	![image_15](/images/how-to-run-cryptonets-in-2022/2022-02-24-153204.png)
	
   5. Put the MNIST dataset files into ```./bin/x64/Debug/```.
	![image_16](/images/how-to-run-cryptonets-in-2022/2022-02-21-205913.png)
	
4. **Run CryptoNets** (the folloing steps are also under CryptoNets Repo)
   1. Open CMD and cd to ```./bin/x64/Debug```.
   2. Execute ```DataPreprocess.exe MNIST``` to generate ```MNIST-28-28-test.txt``` for CryptoNets.exe.
	![image_17](/images/how-to-run-cryptonets-in-2022/2022-02-21-210014.png)
   3. Execute ```CryptoNets.exe```. This program will train a CNN model first and start to infer images in the encrypted domain. 
	![image_18](/images/how-to-run-cryptonets-in-2022/2022-02-21-210425.png)


# Conclusion
This post showed how to run CryptoNets with visual studio 2022. If you are interested in this research topic, there are other applications you can also try in the CryptoNets repo.

# References
1. [CryptoNets](https://proceedings.mlr.press/v48/gilad-bachrach16.html)
2. [My fork of Microsoft SEAL Library](https://github.com/whcjimmy/SEAL/tree/CryptoNets_with_vs_2022)
3. [The offical repo of SEAL](https://github.com/microsoft/SEAL)
4. [The offical repo of CryptoNets](https://github.com/microsoft/CryptoNets)
