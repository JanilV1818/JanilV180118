Display Utility Layers using ArcGIS JavaScript API

<script src="https://js.arcgis.com/4.29/"></script>
<div id="viewDiv" style="height:600px;"></div>
<script>
require(["esri/Map", "esri/views/MapView", "esri/layers/FeatureLayer"], 
function(Map, MapView, FeatureLayer) {
  const map = new Map({ basemap: "topo-vector" });

  const view = new MapView({
    container: "viewDiv",
    map: map,
    center: [77.1025, 28.7041], 
    zoom: 14
  });

  const waterLines = new FeatureLayer({
    url: "https://<your-server>/arcgis/rest/services/Utilities/WaterLines/FeatureServer/0"
  });
  map.add(waterLines);
});

</script>

Custom Query via C#.NET Backend (Web API)

public IHttpActionResult GetValvesByZone(string zoneId)
{
    using (SqlConnection conn = new SqlConnection(ConfigurationManager.ConnectionStrings["GISDB"].ConnectionString))
    {
        conn.Open();
        SqlCommand cmd = new SqlCommand("SELECT * FROM Valves WHERE ZoneID = @ZoneID", conn);
        cmd.Parameters.AddWithValue("@ZoneID", zoneId);

        SqlDataReader reader = cmd.ExecuteReader();
        List<Valve> valves = new List<Valve>();
        while (reader.Read())
        {
            valves.Add(new Valve
            {
                ValveID = reader["ValveID"].ToString(),
                Latitude = Convert.ToDouble(reader["Latitude"]),
                Longitude = Convert.ToDouble(reader["Longitude"])
            });
        }
        return OK(valves);
      
    }
}

ArcGIS Silverlight Viewer - Add Custom Button (Legacy)

<esri:Toolbar>
  <esri:ToolbarItem Name="CustomButton" IconSource="Icons/custom.png" 
      Click="CustomButton_Click" ToolTip="Check Valve Info"/>
</esri:Toolbar>

private void CustomButton_Click(object sender, RoutedEventArgs e)
{
    MessageBox.Show("Custom field verification logic can be added here");
}

CREATE TABLE ElectricPoles (
    PoleID INT PRIMARY KEY,
    Latitude FLOAT,
    Longitude FLOAT,
    InstalledDate DATE,
    Condition VARCHAR(50),
    Verified BIT
);
