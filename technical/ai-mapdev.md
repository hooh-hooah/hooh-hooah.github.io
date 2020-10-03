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
