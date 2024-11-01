# List-View
md command-extension
cd command-extension
yo @microsoft/sharepoint
code .
{
  "$schema": "https://developer.microsoft.com/json-schemas/spfx/command-set-extension-manifest.schema.json",

  "id": "95688e19-faea-4ef1-8394-489bed1de2b4",
  "alias": "HelloWorldCommandSet",
  "componentType": "Extension",
  "extensionType": "ListViewCommandSet",

  "version": "*",
  "manifestVersion": 2,

  "requiresCustomScript": false,

  "items": {
    "COMMAND_1": {
      "title": { "default": "Command One" },
      "iconImageUrl": "icons/request.png",
      "type": "command"
    },
    "COMMAND_2": {
      "title": { "default": "Command Two" },
      "iconImageUrl": "icons/cancel.png",
      "type": "command"
    }
  }
}
import { Log } from '@microsoft/sp-core-library';
import {
  BaseListViewCommandSet,
  Command,
  IListViewCommandSetListViewUpdatedParameters,
  IListViewCommandSetExecuteEventParameters
} from '@microsoft/sp-listview-extensibility';
import { Dialog } from '@microsoft/sp-dialog';
@override
public onListViewUpdated(event: IListViewCommandSetListViewUpdatedParameters): void {
  const compareOneCommand: Command = this.tryGetCommand('COMMAND_1');
  if (compareOneCommand) {
    // This command should be hidden unless exactly one row is selected.
    compareOneCommand.visible = event.selectedRows.length === 1;
  }
}
@override
public onExecute(event: IListViewCommandSetExecuteEventParameters): void {
  switch (event.itemId) {
    case 'COMMAND_1':
      Dialog.alert(`${this.properties.sampleTextOne}`);
      break;
    case 'COMMAND_2':
      Dialog.alert(`${this.properties.sampleTextTwo}`);
      break;
    default:
      throw new Error('Unknown command');
  }
}
{
  "$schema": "https://developer.microsoft.com/json-schemas/spfx-build/spfx-serve.schema.json",
  "port": 4321,
  "https": true,
  "serveConfigurations": {
    "default": {
      "pageUrl": "https://sppnp.sharepoint.com/sites/Group/Lists/Orders/AllItems.aspx",
      "customActions": {
        "bf232d1d-279c-465e-a6e4-359cb4957377": {
          "location": "ClientSideExtension.ListViewCommandSet.CommandBar",
          "properties": {
            "sampleTextOne": "One item is selected in the list",
            "sampleTextTwo": "This command is always visible."
          }
        }
      }
    },
    "helloWorld": {
      "pageUrl": "https://sppnp.sharepoint.com/sites/Group/Lists/Orders/AllItems.aspx",
      "customActions": {
        "bf232d1d-279c-465e-a6e4-359cb4957377": {
          "location": "ClientSideExtension.ListViewCommandSet.CommandBar",
          "properties": {
            "sampleTextOne": "One item is selected in the list",
            "sampleTextTwo": "This command is always visible."
          }
        }
      }
    }
  }
}
gulp serve
@override
public onExecute(event: IListViewCommandSetExecuteEventParameters): void {
  switch (event.itemId) {
    case 'COMMAND_1':
      Dialog.alert(`Clicked ${strings.Command1}`);
      break;
    case 'COMMAND_2':
      Dialog.prompt(`Clicked ${strings.Command2}. Enter something to alert:`).then((value: string) => {
        Dialog.alert(value);
      });
      break;
    default:
      throw new Error('Unknown command');
  }
}
<?xml version="1.0" encoding="utf-8"?>
<Elements xmlns="http://schemas.microsoft.com/sharepoint/">
    <CustomAction
        Title="SPFxListViewCommandSet"
        RegistrationId="100"
        RegistrationType="List"
        Location="ClientSideExtension.ListViewCommandSet.CommandBar"
        ClientSideComponentId="5fc73e12-8085-4a4b-8743-f6d02ffe1240"
        ClientSideComponentProperties="{&quot;sampleTextOne&quot;:&quot;One item is selected in the list.&quot;, &quot;sampleTextTwo&quot;:&quot;This command is always visible.&quot;}">
    </CustomAction>
</Elements>
<?xml version="1.0" encoding="utf-8"?>
<Elements xmlns="http://schemas.microsoft.com/sharepoint/">
    <CustomAction
        Title="SPFxListViewCommandSet"
        RegistrationId="100"
        RegistrationType="List"
        Location="ClientSideExtension.ListViewCommandSet.CommandBar"
        ClientSideComponentId="5fc73e12-8085-4a4b-8743-f6d02ffe1240"
        ClientSideComponentProperties="{&quot;sampleTextOne&quot;:&quot;One item is selected in the list.&quot;, &quot;sampleTextTwo&quot;:&quot;This command is always visible.&quot;}">
    </CustomAction>
    <CustomAction
        Title="SPFxListViewCommandSet"
        RegistrationId="101"
        RegistrationType="List"
        Location="ClientSideExtension.ListViewCommandSet.CommandBar"
        ClientSideComponentId="5fc73e12-8085-4a4b-8743-f6d02ffe1240"
        ClientSideComponentProperties="{&quot;sampleTextOne&quot;:&quot;One item is selected in the list.&quot;, &quot;sampleTextTwo&quot;:&quot;This command is always visible.&quot;}">
    </CustomAction>
</Elements>
{
  "$schema": "https://developer.microsoft.com/json-schemas/spfx-build/package-solution.schema.json",
  "solution": {
    "name": "command-extension-client-side-solution",
    "id": "0abe5c73-1655-49d3-922b-7a47dd70e151",
    "version": "1.0.0.0",
    "includeClientSideAssets": true,
    "isDomainIsolated": false,
    "features": [
      {
        "title": "Application Extension - Deployment of custom action.",
        "description": "Deploys a custom action with ClientSideComponentId association",
        "id": "25f8df47-61f2-4d75-bfe2-8d614f775219",
        "version": "1.0.0.0",
        "assets": {
          "elementManifests": [
            "elements.xml",
            "clientsideinstance.xml"
          ]
        }
      }
    ]
  },
  "paths": {
    "zippedPackage": "solution/command-extension.sppkg"
  }
}
