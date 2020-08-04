# React Tizen

This project is based on [Create React App](https://github.com/facebook/create-react-app) and demonstrates a simple React project for Samsung Tizen TVs.

There are a few steps to make this project work on a Tizen-compatible TV. First, we will see how to connect your TV to the development environment and allow applications to be installed on it. Then, we will see how to build this project and install it on the TV.

## Configuring Tizen Studio and your device

- Install [Tizen Studio](https://developer.tizen.org/development/tizen-studio/download)
- In Tizen Studio, use the Package Manager (Tools › Package Manager or <kbd>Alt + Shift + P</kbd>) to install "Samsung Certificate Extension" and "TV Extensions" (you can find them in the "Extension SDK" tab)
- Close the Package Manager and launch Tizen Studio (it might request you to set your workspace folder, you can choose any folder you want)
- Connect your TV to Tizen Studio by following [these instructions](https://developer.samsung.com/smarttv/develop/getting-started/using-sdk/tv-device.html).
- Generate a new Samsung certificate using the Certificate Manager (Tools › Certificate Manager or <kbd>Alt + Shift + C</kbd>).
  - Select "Samsung" certificate
  - Select "TV" device type
  - Select "Create a new certificate profile" and name it (we will need this name later)
  - Select "Create a new author certificate"
  - Input a name and a password
  - It will ask you to connect or create a Samsung Account.
  - If you want, select a backup path for the certificate and click next
  - Select "Create a new distributor certificate"
  - Make sure the connected TV DUID is in the DUID list and click next
  - Click finish and close the Certificate Manager
- Go to the Device Manager (Tools › Device Manager or <kbd>Alt + Shift + V</kbd>), right-click the emulator and click "Permit to install applications". You should be prompted with "Suceeded to upload a certificate". You can then close the device manager.

## Building and installing this project

- Make sure that the `tizen` or `tizen.bat` CLI is available in your shell path
- Create a `config.xml` in the `tizen` folder. This can be done by creating a new Tizen project in Tizen Studio.
  - Create a new Tizen Project
  - Select "Template"
  - Select "TV"
  - Select "Web Application"
  - Select "Empty" project
  - Choose a name and click finish
  - Once the project is generated, copy the `config.xml` file to the `tizen` folder of this project
- Before building the project for the first time, edit the `build` and `deploy` scripts in the `package.json` file:
  - Replace `tizen.bat` with `tizen` if necessary (if you are running a unix system)
  - In the `tizen.bat package -t wgt -s default -- build/.buildResult` command, replace `default` with the name you gave to your certificate
  - In the `tizen.bat install -n ReactTizen.wgt -- build/.buildResult` command, replace `ReactTizen.wgt` with the name you gave to your application in the `config.xml`
- Run the following command to build the project:
  ```bash
  yarn build
  ```
  This will perform the following actions:
  - Build the React project in the `build` folder
  - Copy the Tizen `config.xml` file from the `tizen` folder to the `build` folder
  - Build the `build` folder as a Tizen web app in the `build/.buildResult` folder
  - Package and sign the app as a `*.wgt` file
- Run the following command to install the project on the connect TV:
  ```bash
  yarn deploy
  ```
  This will upload the `*.wgt` to the device and install it. You can then find the app in the "Apps" section of your TV to launch it.
