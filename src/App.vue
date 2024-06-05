<template>
  <ejs-diagram :width="width" :height="height" 
  :nodes="nodes" :connectors="connectors"
  :pageSettings="pageSettings"
  :snapSettings="snapSettings"
  :getNodeDefaults="getNodeDefaults"
  :getConnectorDefaults="getConnectorDefaults"
  :constraints="constraints"
  :bridgeDirection="bridgeDirection"
  ></ejs-diagram>
</template>

<script>
// #region Nodes and Connectors definitions
const nodes = [
  {
    id: "startNode",
    offsetX: 300,
    offsetY: 60,
    height: 60,
    width: 150,
    shape: { type: "Flow", shape: "Terminator" },
    annotations: [{ content: 'Start'}]
  },
  {
    id: "inputNode",
    offsetX: 300,
    offsetY: 180,
    height: 60,
    width: 150,
    shape: { type: "Flow", shape: "Data" },
    annotations: [{ content: 'Enter a number'}]
  },
  {
    id: "decisionNode",
    offsetX: 300,
    offsetY: 300,
    height: 60,
    width: 150,
    shape: { type: "Flow", shape: "Decision" },
    annotations: [{ content: 'N divisible by 2 ?'}]
  },
  {
    id: "processEvenNode",
    offsetX: 600,
    offsetY: 300,
    height: 60,
    width: 150,
    shape: { type: "Flow", shape: "Process" },
    annotations: [{ content: 'N is Even'}]
  },
  {
    id: "processOddNode",
    offsetX: 300,
    offsetY: 420,
    height: 60,
    width: 150,
    shape: { type: "Flow", shape: "Process" },
    annotations: [{ content: 'N is Odd'}]
  },
  {
    id: "endNode",
    offsetX: 300,
    offsetY: 540,
    height: 60,
    width: 150,
    shape: { type: "Flow", shape: "Terminator" },
    annotations: [{ content: 'End' }]
  }
];

const connectors = [
  {
    id: 'startToInputConnector',
    sourceID: 'startNode',
    targetID: 'inputNode'
  },
  {
    id: 'inputToDecisionConnector',
    sourceID: 'inputNode',
    targetID: 'decisionNode'
  },
  {
    id: 'decisionToProcessEvenConnector',
    sourceID: 'decisionNode',
    targetID: 'processEvenNode',
    annotations: [{ content: 'true', alignment: 'Before', displacement: { x: 3, y: 3 } }]
  },
  {
    id: 'decisionToProcessOddConnector',
    sourceID: 'decisionNode',
    targetID: 'processOddNode',
    annotations: [{ content: 'false', alignment: 'After', displacement: { x: 5, y: 5 } }]
  },
  {
    id: 'processOddToEndConnector',
    sourceID: 'processOddNode',
    targetID: 'endNode'
  },
  {
    id: 'processEvenToEndConnector',
    sourceID: 'processEvenNode',
    targetID: 'endNode',
    type: "Orthogonal",
    segments: [{ type: "Orthogonal", direction: "Bottom", length: 200 }]
  }
];
// #endregion

import {
  DiagramComponent, SnapConstraints, ConnectorBridging, DiagramConstraints
} from '@syncfusion/ej2-vue-diagrams';

export default {
  components: {
    'ejs-diagram': DiagramComponent
  },
  data: function () {
    return {
      width: '902px',
      height: '602px',
      nodes: nodes,
      connectors: connectors,
      snapSettings: {constraints: SnapConstraints.None },
      constraints: DiagramConstraints.Default | DiagramConstraints.Bridging,
      bridgeDirection: 'Right',
      pageSettings:{
        height: 842,
        width: 595,
        showPageBreaks: true,
        margin: { left: 20, right: 20, top: 20, bottom: 20 },
        background: { color: '#CEF6F5' },
        orientation: 'Portrait',
        multiplePage: true
      } 
    };
  },
  methods: {
    getNodeDefaults(node){
      //node.pivot = { x:0, y:0 };
      node.style ={ fill: 'LightGreen', strokeColor: 'Orange', strokeWidth: 3, strokeDashArray: '7,7',
       gradient: {
          type: 'Linear',
          x1: 0,
          y1: 0,
          x2: 50,
          y2: 50,
          stops:[
            { color: 'White', offset: 0 },
            { color: '#6BA5D7', offset: 100 }
          ]
        } 
       };
    },
    getConnectorDefaults(connector){
      connector.style = { strokeColor: 'Blue' };
      connector.sourceDecorator = { shape: 'Diamond', style: { fill: 'Yellow', strokeColor: 'Blue', strokeWidth: 2 }};
      connector.targetDecorator = { shape: 'IndentedArrow'};
      connector.hitPadding = 50;
      connector.sourcePadding = 5;
      connector.targetPadding = 5;
    }
  },
  provide: { diagram:[ConnectorBridging]}
};
</script>

<style>
@import "../node_modules/@syncfusion/ej2-vue-diagrams/styles/material.css";
</style>