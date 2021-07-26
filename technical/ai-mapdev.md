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
        // when the map instance is valid and the map id is different compared to the current map
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

### Map Change Process

This process is recreated by me with following the callstack of `LoadMap()`

#### Request change the map

```csharp
Singleton<Game>.Instance.WorldData.MapID = mapID;
Observable.FromCoroutine(() => this.ChangeMapCoroutine(mapID, onEndFadeIn, onCompleted), false).Subscribe<Unit>();
```

#### change the map with the coroutine

```csharp
private IEnumerator ChangeMapCoroutine(int mapID, Action onEndFadeIn = null, Action onCompleted = null) {
    // hidden: Initialize the loading panel
    // hidden: make loading panel visible with easing.
    // hidden: Release animal information is AnimalManager instance is valid.
	this.ReleaseMap();
	this.ReleaseAgents();
	this.Player.DisableEntity();
	this.Merchant.StopBehavior();
	this.Merchant.DisableEntity();
	this.Merchant.DeactivateNavMeshElement();
	yield return this.LoadMap(mapID);
	yield return this.LoadNavMeshSource();
	yield return this.LoadElements();
	yield return this.LoadMerchantPoint();
	yield return this.LoadEventPoints();
	yield return this.LoadStoryPoints();
	yield return this.LoadHousingObj(mapID);
	this.SetVanishList();
	this.BuildNavMesh();
	this._pointAgent.CreateHousingWaypoint();
	yield return this.LoadAnimalPoint();
	yield return this.SetupPoint();
	AnimalManager animalManager = Singleton<AnimalManager>.Instance;
	yield return animalManager.SetupPointsAsync(this._pointAgent);
    // is soundplayer manager instance is valid, refreshjukebox table.
	yield return this.RefreshAsync();
	WorldData worldData = Singleton<Game>.Instance.WorldData;
	bool existsBackup = Singleton<Game>.Instance.ExistsBackup(worldData.WorldID);
	yield return this.LoadAgents(worldData, existsBackup);
	this.RefreshAgentStatus();
	this.RefreshAnimalStatus();
	yield return null;
	this.RefreshActiveTimeRelationObjects();
	this.InitActionPoints();
	this.Player.PlayerController.CommandArea.InitCommandStates();
	this.InitCommandable();
	this.AddAppendTargetPoints(Singleton<Housing>.Instance.ActionPoints);
	this.AddRuntimeFarmPoints(Singleton<Housing>.Instance.FarmPoints);
	this.AddPetHomePoints(Singleton<Housing>.Instance.PetHomePoints);
	this.AddJukePoints(Singleton<Housing>.Instance.JukePoints);
	this.AddRuntimeCraftPoints(Singleton<Housing>.Instance.CraftPoints);
	this.AddRuntimeLightSwitchPoints(Singleton<Housing>.Instance.LightSwitchPoints);
	this.AddAppendHPoints(Singleton<Housing>.Instance.HPoints);
	foreach (ActionPoint actionPoint in Singleton<Housing>.Instance.ActionPoints)
	{
		OffMeshLink[] componentsInChildren = actionPoint.GetComponentsInChildren<OffMeshLink>();
		foreach (OffMeshLink offMeshLink in componentsInChildren)
		{
			offMeshLink.UpdatePositions();
		}
	}
	this.LoadAgentTargetActionPoint();
	this.LoadAgentTargetActor();
	WorldData worldData2 = Singleton<Game>.Instance.WorldData;
	bool existsBackup2 = Singleton<Game>.Instance.ExistsBackup(worldData2.WorldID);
	yield return this.LoadAnimals(worldData2, existsBackup2);
	this.RefreshAreaOpenLinkedObject();
	this.RefreshTimeOpenLinkedObject();
    // hidden: check sound data from the config.sounddata.sounds if sound data is null then refresh.
	yield return this.LoadEnviroObject();
	MapArea mapArea = this.Player.MapArea;
	int? num = (mapArea != null) ? new int?(mapArea.AreaID) : null;
	this.ResettingEnviroAreaElement(mapID, (num == null) ? 0 : num.Value);
	MapArea mapArea2 = this.Player.MapArea;
	int? num2 = (mapArea2 != null) ? new int?(mapArea2.AreaID) : null;
	this.RefreshHousingEnv3DSEPoints(mapID, (num2 == null) ? 0 : num2.Value);
	yield return new global::WaitForSecondsRealtime(1f);
	WaitForSeconds waitTime = new WaitForSeconds(0.5f);
	while (!this.WaitCompletionAgents())
	{
		yield return waitTime;
	}
	this._agentVanishAreaList = null;
    // iterate agent with this._agentTable.Values and ActivateNavMeshAgent
	yield return null;
    // iterate agent with this._agentTable.Values and CalculateWaypoints
	this.Player.ActivateNavMeshAgent();
	this.Merchant.SetMerchantPoints(this._pointAgent.MerchantPoints);
	this.Merchant.ChangeMap(mapID);
	MapUIContainer.InitializeMinimap();
	animalManager.StartAllAnimalCreate();
	yield return Singleton<Resources>.Instance.HSceneTable.LoadHObj();
    // mark animal manager activemapscene.
    // collect illusion :^)
	this.Merchant.EnableEntity();
	this.Merchant.FirstAppear();
	this.Player.EnableEntity();
    // move player to the startpoint
	Transform startPoint = this._pointAgent.ShipPoints[0].StartPointFromMigrate;
	this.Player.NavMeshAgent.Warp(startPoint.position);
	this.Player.Rotation = startPoint.rotation;

	this.InitSearchActorTargetsAll();
    // iterate this._agentTable and calculate the behavior again.
	foreach (KeyValuePair<int, AgentActor> keyValuePair in this._agentTable)
	this.Player.PlayerController.CommandArea.InitCommandStates();
	this.Player.PlayerController.CommandArea.RefreshCommands();
    // fadeout loading screen.
    // yield null if timescale is zero.
    // give an achievement
    // invoke complete delegate
	yield break;
}
```

#### invoke the oncomplete delegate (if player is moving out from the boat.)

```csharp
// "that oncomplete delegate
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
```
