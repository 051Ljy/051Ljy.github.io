<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>WebGIS应用</title>
    <link rel="stylesheet"
     href="https://js.arcgis.com/4.32/esri/themes/light/main.css">
     <script src="https://js.arcgis.com/4.32/"></script>
    <style>
        body {
            margin: 0;
            padding: 0;
            display: flex;
            flex-flow: column;
            height: 100vh;
        }
        
        #header {
            height: 60px;
            background: #dbdb21;
            color: white;
            padding: 10px;
            display: flex;
            align-items: center;
        }
        
        #mainContainer {
            flex: 1;
            display: flex;
        }
        
        #mapView {
            flex: 3;
        }
        
        #basemapGallery {
            flex: 1;
            padding: 10px;
            background: #f5f5f5;
            overflow-y: auto;
        }
        
        .basemap-thumbnail {
            width: 100%;
            height: 150px;
            margin-bottom: 10px;
            cursor: pointer;
            border: 2px solid transparent;
        }
        
        .basemap-thumbnail:hover {
            border-color: #0079c1;
        }
    </style>
</head>
<body>
    <div id="header">
        <h1>WebGIS应用演示</h1>
        <div id="searchWidget" style="margin-left: auto; margin-right: 20px;"></div>
        <div>
            <p>数据来源:贵州省旅游文化厅</p>
          </div>
    </div>
    <div id="mainContainer">
        <div id="mapView"></div>
     <!-- <div id="basemapGallery">
       <h3 style="margin: 0 0 10px 10px;">底图选择</h3>
        </div class="basemap-item" data-basemap="streets-vector"></div> 
        </div class="basemap-item" data-basemap="satellite">卫星影像</div> 
        </div class="basemap-item" data-basemap="topo">地形图</div> 
        </div>-->
    </div>
    <script>
        require([
        "esri/Map",
      "esri/views/MapView",
      "esri/layers/FeatureLayer",
      "esri/widgets/ScaleBar",
      "esri/widgets/Legend",
      "esri/widgets/Search",
      "esri/widgets/LayerList",
      "esri/widgets/Compass",
      "esri/widgets/NavigationToggle",
      "esri/widgets/Home",
      "esri/widgets/BasemapGallery",
      "esri/Basemap"
        ], function(
            Map, MapView, FeatureLayer, ScaleBar, Legend, Search,
             LayerList, Compass, NavigationToggle, Home, BasemapGallery, Basemap
        ) {
            
            
            // 初始化地图
            
             const map = new Map({
            basemap: "streets-vector" 
        });

            // 添加专题图层（示例使用ArcGIS Online的公共图层）
            const featureLayer = new FeatureLayer({
                url: "https://www.geosceneonline.cn/server/rest/services/Hosted/贵州省A级景区分布/FeatureServer/0"
            });
            map.add(featureLayer);

            // 初始化地图视图
            const view = new MapView({
                container: "mapView",
                map: map,
                center: [106.42, 26.35], // 贵州坐标
                zoom: 7
            });

            // 添加比例尺
            const scaleBar = new ScaleBar({
                view: view,
                unit: "metric"
            });
            view.ui.add(scaleBar, {
                position: "bottom-left"
            });

            // 添加图例
            const legend = new Legend({
                view: view,
                layerInfos: [{
                    layer: featureLayer,
                    title: "贵州省A级景区分布"
                }]
            });
            view.ui.add(legend, "top-left");

            // 添加搜索框
            const searchWidget = new Search({
                view: view
            });
            view.ui.add(searchWidget, {
                position: "top-right",
                index: 0
            });

            // 初始化底图库
            const basemapGallery = new BasemapGallery({
        view: view,
        container: document.createElement("div"),
        source: {
          portal: {
            url: "https://www.arcgis.com",
            useVectorBasemaps: true
          }
        }
      });
      
      // 将底图库添加到视图UI
      view.ui.add(basemapGallery, {
        position: "bottom-right",
        index: 0
      });

            // 响应式布局处理
            view.watch("widthBreakpoint", breakpoint => {
                const gallery = document.getElementById("basemapGallery");
                gallery.style.display = breakpoint === "xsmall" ? "none" : "block";
            });
        });
    </script>
</body>
</html>
