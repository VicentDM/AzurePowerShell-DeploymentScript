Copyright [2021] [Vicent Dolz Martinez]

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

Import-Module -Name "Az"
Import-Module -Name "Az.Resources"
Import-Module -Name "Az.datafactory"
Get-Module "*datafactory*"

#Set global variables
$resourceGroupName = "RG-STG-BI"
$dataFactoryName = "AZ-STG-BI-DF01"
$region = "westeurope"

#Get templates path
$scriptPath = (Join-Path -Path (Get-Location) -ChildPath "\_BI ADF PB\bi-dev-datafactory\linkedTemplates")

#Count the total number of linked templates and set up max_value
$a = Get-ChildItem -Path $scriptPath -File -Filter 'ArmTemplate_*' | ? {
  $_.Name -match 'ArmTemplate_(\d+)\.json' }
$max_value = $a.Count

#if statement to avoid error
if($max_value -gt 0){
    #Print total number of templates
    Write-Host "There are $max_value ArmTemplates"

    #Loop to deploy each template
    for ($num = 0; $num -lt $max_value; $num++ ){
        Write-Host "Deploying ArmTemplate_$num.json"
        $deploymentFilePath = "$scriptPath\ArmTemplate_$num.json"
        #Read json template, override the previous dataFactoryName and write into the same json template file  
        (Get-Content $deploymentFilePath).Replace("bi-dev-datafactory", $dataFactoryName) | Set-Content $deploymentFilePath
        #Deploy command
        New-AzResourceGroupDeployment -Name $dataFactoryName -ResourceGroupName $resourceGroupName -TemplateFile $deploymentFilePath -verbose
    }
}else {
    #Throw an error if no templates files are found
    Throw "Error: No ArmTemplates found in this folder!"
}
