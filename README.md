Alexa Lambda Deployment
===============

To deploy a Node.js sample as a Lambda function, you first need to create a zip file containing the .js files provided in the sample. If the project uses the Alexa Skills Kit SDK for Node.js (which we may have), you need to also include these dependencies in your zip file.

In a terminal or command line, navigate to the `src` directory for the sample and enter this command:
bash npm install --save alexa-sdk
This installs a node_modules directory containing the SDK dependencies.

Select all the contents of the src folder, and create a zip file.
The zip should contain the files, but not include the src folder. The index.js file must be at the root of the zip. The zip should include the node_modules directory.


## Creating your new Lambda function and uploading the code.

Once you have either a zip containing the Node.js code, do the following:
1. Log in to the AWS Management Console and navigate to AWS Lambda.
2. Create a new Lambda function in the US East (N. Virginia) or EU (Ireland) region.
3. To quickly set up one of the blueprint samples, select the sample from the list of blueprints. The following samples are available as blueprints:
  * alexa-skill-kit-sdk-factskill
  * alexa-skill-kit-sdk-howtoskill
  * alexa-skill-kit-sdk-triviaskill
  * alexa-skills-kit-color-expert
Selecting one of these blueprints automatically imports the code into the Lambda console.
4. Configure the new function with the following settings, depending on whether your are deploying a Node.js or Java sample:

| Setting       | Value         |
| ------------- | ------------- |
| Triggers      | Click the outlined box select Alexa Skills Kit.  |
| Name          | A descriptive name for the function.  |
| Description   | A description for the function.  |
| Runtime       | Node.js  |
| Lambda Function Code  | 	Select the Upload a .ZIP file option and upload the zip file you created. If you selected one of the blueprints (such as alexa-skill-kit-sdk-factskill), the code is already filled in.|
| Handler       |  index.handler |

### Select the Role for the function. This defines the AWS resources the function can access.

To use an existing role, select the role under `Use existing role`.
<br>
<br>
**otherwise, to create a new role..**
<br>
### Defining a New Role for the Function

The role specifies the AWS resources your function can access. To create a new role while configuring your function:
For Role (under Lambda function handler and role), select Create new role from template(s).
Enter the Role Name.
From the Policy templates list, select Simple Microservice permissions.

#### Make note of the Amazon Resource Name (ARN) for your new Lambda function. The ARN is displayed in the upper-right corner of the function page. COPY this somewhere, we will need it on one of the final steps.

## Creating a New Skill on the Developer Portal

- Register a new Alexa skill on the developer portal.
- For details, see [Registering and Managing Custom Skills in the Dev Portal](https://developer.amazon.com/public/solutions/alexa/alexa-skills-kit/docs/registering-and-managing-alexa-skills-in-the-developer-portal)
- Use the following information when registering the new skill:

<br>

| Setting |	Value |
| ------- | ----- |
| Invocation | Name	any valid invocation name. |
| Name |	Any valid name. |
| Endpoint |	Select the AWS Lambda ARN (Amazon Resource Name) option, then either North America or Europe and enter the ARN for your function. |
| Interaction Model |	Each sample includes an interaction model in the speechAssets folder for the sample. <br><br>Copy the JSON from IntentSchema.json into the Intent Schema box and copy the text from SampleUtterances.txt into the Sample Utterances box. <br><br>For details about defining the interaction model, see Define the Interaction Model in JSON and Text. |





<br>
<br>
https://developer.amazon.com/alexa-skills-kit/alexa-skill-quick-start-tutorial