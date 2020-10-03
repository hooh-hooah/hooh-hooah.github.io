# Map Development Analysis

## AI-Shoujyo Analysis

### Map Resource Locations

| Location                                    | Description                                                                                              |
| ------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| list/actor/agent/vanish/                    | `LoadAgentVanishAreaList`                                                                                |
| list/actor/player/camera/\*.unity3d         | `LoadActionCameraData`                                                                                   |
| list/gameitem/ivy_filter/\*.unity3d         | `LoadIvyFilterList`                                                                                      |
| list/map/actionpoint/agent/\*.unity3d       | `LoadAgentActionPointInfo`, `LoadAgentDateActionPointInfo`                                               |
| list/map/actionpoint/basepoint/\*.unity3d   | `LoadBasePointListTable`                                                                                 |
| list/map/actionpoint/devicepoint/\*.unity3d | `LoadDevicePointListTable`                                                                               |
| list/map/actionpoint/farmpoint/\*.unity3d   | `LoadFarmPointListTable`                                                                                 |
| list/map/actionpoint/info/\*.unity3d        | `LoadActionPointListTable`                                                                               |
| list/map/actionpoint/lightpoint/\*.unity3d  | `LoadLightSwitchPointList`                                                                               |
| list/map/actionpoint/merchant/\*.unity3d    | `LoadMerchangePoint`                                                                                     |
| list/map/actionpoint/player/\*.unity3d      | `LoadPlayerActionPointInfo`, `LoadPlayerDateActionPointInfo`                                             |
| list/map/actionpoint/shippoint/\*.unity3d   | `LoadShipPointListTable`                                                                                 |
| list/map/area/\*.unity3d                    | `LoadAreaGroup`                                                                                          |
| list/map/chunk/\*.unity3d                   | `LoadChunkList`, `LoadSEMeshList`, `LoadCameraColliderList`, `LoadMapGroupList`, `LoadMapHiddenAreaList` |
| list/map/enviro/\*.unity3d                  | `LoadEnviroInfoList`                                                                                     |
| list/map/event_item/\*.unity3d              | `LoadEventItemList`, `LoadFoodEventItemList`                                                             |
| list/map/eventpoint/\*.unity3d              | `LoadEventPointListTable`                                                                                |
| list/map/mapinfo\*.unity3d                  | `LoadMapList`, `LoadNavMeshSourceTable`                                                                  |
| list/map/openstate/\*.unity3d               | `LoadAreaOpenStateList`                                                                                  |
| list/map/plant_item/\*.unity3d              | `LoadPlantItemList`                                                                                      |
| list/map/storypoint/\*.unity3d              | `LoadStoryPointListTable`                                                                                |
| list/map/timeinfo/\*.unity3d                | `LoadTimeRelationInfoLIst`                                                                               |
| list/map/vanish/\*.unity3d                  | `LoadVanishHousingGroup`                                                                                 |
| list/particle/action/\*.unity3d             | `LoadEventParticleList`                                                                                  |
| list/map/minimap/\*.unity3d                 | `LoadMiniMapInfo`                                                                                        |

### Map References

### Getting the map's name

AllAreaMapUI.cs

```csharp
if (Singleton<Manager.Resources>.Instance.Map.MapList.TryGetValue(mapID, out assetBundleInfo))
{
	this.IslandName.text = assetBundleInfo.name;
}
else
{
	this.IslandName.text = "廃墟の島";
}
```

### Data Strcture

#### Map Information

The map information is listed with `ExcelData`. which can be integrated with the sideloader plugin.

has 5 columns:
ID, element1, element2, element3, element4

inserts the data to the `Singleton<Manager.Resources>.Instnace.Map.MapList[key] = value`
`Manager.Resources.MapTables = Singleton<Manager.Resources>.Instnace.Map`

### Map Change Dialog

```csharp
foreach (KeyValuePair<int, AssetBundleInfo> current in Singleton<Manager.Resources>.Instance.Map.MapList)
{
	int id = current.Key;
	string mapName = current.Value.name;
	CommCommandList.CommandInfo item = new CommCommandList.CommandInfo(mapName)
	{
        // when the map id is valid
		Condition = (() => Singleton<Manager.Map>.IsInstance() && id != Singleton<Manager.Map>.Instance.MapID),
		Event = delegate(int x)
		{
            // play ok sound
			Singleton<Manager.Resources>.Instance.SoundPack.Play(SoundPack.SystemSE.OK_S);
            // check the user wants to change the map
			ConfirmScene.Sentence = string.Format("{0}に移動しますか？", mapName);
            // setup yes click event
			ConfirmScene.OnClickedYes = delegate
			{
                // on click yes - play nice sound
				Singleton<Manager.Resources>.Instance.SoundPack.Play(SoundPack.SystemSE.OK_L);
                // clear all the command list
				MapUIContainer.SetActiveCommandList(false);
				this.SetScheduledInteractionState(false);
				this.ReleaseInteraction();
				MapUIContainer.SetCommandLabelAcception(CommandLabel.AcceptionState.None);

                // save map profile
				Singleton<MapScene>.Instance.SaveProfile(true);

                // invoke change map
				Singleton<Manager.Map>.Instance.ChangeMap(id, null, delegate
				{
                    // after the map load, initialize other stuff
					this.PlayerController.ChangeState("Normal");
					this.CameraControl.EnabledInput = true;
					MapUIContainer.SetVisibleHUD(true);
					MapUIContainer.StorySupportUI.Open();
					if (this.PlayerController.CommandArea != null)
					{
						this.PlayerController.CommandArea.enabled = true;
					}
					MapUIContainer.SetCommandLabelAcception(CommandLabel.AcceptionState.InvokeAcception);
				});
			};
            // setup no click event
			ConfirmScene.OnClickedNo = delegate
			{
                // on click no - just play the sound
				Singleton<Manager.Resources>.Instance.SoundPack.Play(SoundPack.SystemSE.Cancel);
			};
            // load the dialog
			Singleton<Game>.Instance.LoadDialog();
		}
	};
	list.Add(item);
}
```
