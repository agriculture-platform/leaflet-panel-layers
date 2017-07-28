Leaflet Panel Layers
==============

Leaflet Control Layers extended with support groups and icons

This plugin is modified from [Leaflet Control Layers](https://github.com/stefanocudini/leaflet-panel-layers) which only works for Leaflet 0.7.3 before.

If you use the above plugin in Leaflet 1.0+, you'll find the panel do not display anything.

So I modified the above project and here it's completely compatible for Leaflet 1.0+.

**demo:**
[labs.easyblog.it/maps/leaflet-panel-layers](http://labs.easyblog.it/maps/leaflet-panel-layers/)


#Usage

**Panel Item Definition formats**
```javascript
	{
		name: "Bar",
		icon: iconByName('bar'),
		layer: L.geoJson(Bar, {pointToLayer: featureToMarker })
	}
```
```javascript
	{
		layer: {
			type: "geoJson",
			args: [ river ]
		},
	}
```
```javascript
	{
		group: "Title Group",
		collapsed: true,
		layers: [
		...other items...
		]
	}
```

**Multiple active layers with icons**
```javascript
var baseLayers = [
	{
		active: true,
		name: "OpenStreetMap",
		layer: L.tileLayer('http://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png')
	}
];
var overLayers = [
	{
		name: "Drinking Water",
		icon: '<i class="icon icon-water"></i>',
		layer: L.geoJson(WaterGeoJSON)
	},
	{
		active: true,
		name: "Parking",
		icon: '<i class="icon icon-parking"></i>',
		layer: L.geoJson(ParkingGeoJSON)
	}
];
map.addControl( new L.Control.PanelLayers(baseLayers, overLayers) );
```

**Build panel layers from pure JSON Config**
```javascript
var panelJsonConfig = {
    "baselayers": [
        {
            "active": true,
            "name": "Open Cycle Map",
            "layer": {
                "type": "tileLayer",
                "args": [
                    "http://{s}.tile.opencyclemap.org/cycle/{z}/{x}/{y}.png"
                ]
            }
        },
        {
            "name": "Landscape",
            "layer": {
                "type": "tileLayer",
                "args": [
                    "http://{s}.tile3.opencyclemap.org/landscape/{z}/{x}/{y}.png"
                ]
            }
        },        
        {
            "name": "Transports",
            "layer": {
                "type": "tileLayer",
                "args": [
                    "http://{s}.tile2.opencyclemap.org/transport/{z}/{x}/{y}.png"
                ]
            }
        }
    ],
    "overlayers": [
        {
            "name": "Terrain",
            "layer": {
            "type": "tileLayer",
            "args": [
                "http://toolserver.org/~cmarqu/hill/{z}/{x}/{y}.png", {
                "opacity": 0.5
                }
            ]
            }
        }
    ]
};
L.control.panelLayers(panelJsonConfig.baseLayers, panelJsonConfig.overLayers).addTo(map);
```

**Grouping of layers**
```javascript
L.control.panelLayers(
	[
		{
			name: "Open Street Map",
			layer: osmLayer
		},
		{
			group: "Walking layers",
			layers: [
				{
					name: "Open Cycle Map",
					layer: L.tileLayer('http://{s}.tile.opencyclemap.org/cycle/{z}/{x}/{y}.png')
				},
				{
					name: "Hiking",
					layer: L.tileLayer("http://toolserver.org/tiles/hikebike/{z}/{x}/{y}.png")
				}
			]
		},
		{
			group: "Road layers",
			layers: [
				{
					name: "Transports",
					layer: L.tileLayer("http://{s}.tile2.opencyclemap.org/transport/{z}/{x}/{y}.png")
				}
			]
		}
	],
	{collapsibleGroups: true}
).addTo(map);
```

**Collapse some layers' groups**
```javascript
L.control.panelLayers([
	{
		name: "Open Street Map",
		layer: osmLayer
	},
	{
		group: "Walking layers",
		layers: [
			{
				name: "Open Cycle Map",
				layer: L.tileLayer('http://{s}.tile.opencyclemap.org/cycle/{z}/{x}/{y}.png')
			},
			{
				name: "Hiking",
				layer: L.tileLayer("http://toolserver.org/tiles/hikebike/{z}/{x}/{y}.png")
			}			
		]
	},
	{
		group: "Road layers",
		collapsed: true,
		layers: [
			{
				name: "Transports",
				layer: L.tileLayer("http://{s}.tile2.opencyclemap.org/transport/{z}/{x}/{y}.png")
			}
		]
	}
]).addTo(map);
```

**Add layers dynamically at runtime**
```javascript
var panel = L.control.panelLayers();

$.getJSON('some/url/path.geojson', function(data){
	panel.addOverlay({
		name: "Drinking Water",
		icon: '<i class="icon icon-water"></i>',
		layer: L.geoJson(data)
	});
});
```


#Build

This plugin support [Grunt](http://gruntjs.com/) for building process.
Therefore the deployment require [NPM](https://npmjs.org/) installed in your system.
After you've made sure to have npm working, run this in command line:
```bash
npm install
grunt
```
