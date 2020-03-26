# appdynamics-powershell-core-wrapper

I started developing this wrapper to help our customer to use public and internal Appdynamics APIs with Powershell, so everyone can get appdynamics information with no Appdynamics api knowledge.

This Wrapper could help our customers to automate their pipelines, improve integrations and for data extraction.

I have created some scripts using this wrapper for different proposes, and I'm sharing with all, so you can have some ideas how to use.

# Installation

Note that you only need to put the file **Appdynamics.psm1** inside your script folder to use the Module.

# USAGE

## Simple Rest

Using module './Appdynamics.psm1'

$auth = Get-AuthorizationHeader -pair "user@account:password"

$appdy = [Appdynamics]::new("https://customer.saas.appdynamics.com",$auth)

$appID = $appdy.GetAppID("APP_NAME")

Write-Host "AppID : $appID"

$tierID = $appdy.GetTierID($appID, "XP.FixedIncome.Asset.Web")

appdy.SendEvent("New Deployment Diego", "This is a test", "INFO", "VSTS", $tierName, $appID,$tierID)


## Simple Rest-UI

	Using module './Appdynamics.psm1'

	$auth = Get-AuthorizationHeader -pair "user@account:password"

	$appdy = [Appdynamics]::new("https://customer.saas.appdynamics.com",$auth)

	$appdy.GetLogin()

	$appName = "Diego Test 123"

	$appDescription = "Test App"

	$tierName = "TestDiego123"

	$appID = $appdy.CreateApplication($appName,$appDescription)

	$tierID = $appdy.CreateTierNET($appID,$tierName)

	$appdy.GetTiers("21086")

	$appdy.GetNodes($app.id)

	$apps = $appdy.GetApplications()

## Tiers per application

	foreach ($tier in $tiers.tiers.tier) {

		Write-Host "`nID: "$tier.id" `nName: "$tier.name"

	}

## Nodes per application

  

	foreach ($app in $apps.applications.application) {

		Write-Host "`nID: "$app.id" `nName: "$app.name" `nDescription: "$app.description

		$nodes = $appdy.GetNodes($app.id)

		foreach ($node in $nodes.nodes.node) {

			Write-Host ($app.name + "," + $node.name)
		
		}
	}


## Import configs - Rest-ui

	$appdy.GetFileToken()

	$appdy.ImportHealthRules($appID,"health.json")

	$appdy.ImportActions($appID,"actions.json")

	$appdy.ImportPolicies($appID,"policies.json")

	$appdy.UploadAppConfig("App_Template_Config.xml")

	$appdy.ImportAppConfig($appID)

	$appdy.ImportExitPoints($appID,@("Create-TASK.json","Create-GRPC.json"))

## Metrics

	$metrics = $appdy.GetMetricsJSON($appID,"Overall Application Performance|*|*","1600")

## Summary Metrics

	$metrics = $appdy.GetAllTierPerformanceSummary("ALL","1600")

	Write-Host "Append Metrics"

	$metrics2 = $appdy.AppendSummaryMetrics($metrics)

	Write-Host $metrics2 | Out-String

	Write-Host "Creating Report"

	$report = $appdy.CreateTierReportCSV($metrics)

	Write-Host "Exporting to CSV"

	$report | Export-Csv '/Users/dieperei/Documents/Development/Powershell/tiers4.csv' -delimiter "," -force -notypeinformation