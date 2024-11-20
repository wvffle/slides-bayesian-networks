<script setup lang="ts">
import cytoscape from "cytoscape";
import { ref, watchEffect, nextTick } from "vue";
import { onSlideEnter, onSlideLeave, useSlideContext } from "@slidev/client";
import edgehandles from "cytoscape-edgehandles";

cytoscape.use(edgehandles);

const viewer = ref();
const { $clicks: clicks } = useSlideContext();

let allNodes;

let cyRef = ref();
onSlideEnter(async () => {
  await nextTick();
  if (!viewer.value) return;
  const cy = (cyRef.value = cytoscape({
    container: viewer.value,
    pixelRatio: 2,
    style: [
      {
        selector: "node",
        style: { label: "data(id)", "text-valign": "center" },
      },
      {
        selector: "edge",
        style: {
          "curve-style": "bezier",
          "line-color": "#ccc",
          "target-arrow-color": "#ccc",
          "source-arrow-color": "#ccc",
        },
      },
    ],
    layout: {
      name: "preset",
    },
    elements: {
      nodes: [
        { data: { id: "A" } },
        { data: { id: "B" }, position: { x: 0, y: 100 } },
        { data: { id: "C" }, position: { x: 60, y: 70 } },
        { data: { id: "D" }, position: { x: 150, y: 100 } },
      ],
      edges: [
        { data: { id: "AB", source: "A", target: "B" } },
        { data: { id: "AC", source: "A", target: "C" } },
        { data: { id: "AD", source: "A", target: "D" } },
        { data: { id: "BC", source: "B", target: "C" } },
        { data: { id: "BD", source: "B", target: "D" } },
        { data: { id: "CD", source: "C", target: "D" } },
      ],
    },
  }));

  allNodes = cy.filter("*");
  viewer.value.style.width = "50vw";
  viewer.value.style.height = "100vh";

  let lastId = "D";
  const createNode = (position = { x: 50, y: 50 }) => {
    const code = lastId.charCodeAt(0) + 1;
    lastId = String.fromCharCode(code);

    return { data: { id: lastId }, position };
  };

  cy.elements().on("cxttap", function (event) {
    const { target: node } = event;
    node.remove();
  });
});

onSlideLeave(() => {
  cyRef.value.destroy();
  cyRef.value = undefined;
  viewer.value.style.width = "100%";
  viewer.value.style.height = "100%";
});

watchEffect(() => {
  if (!cyRef.value) return;
  const cy = cyRef.value;

  switch (clicks.value) {
    case 0:
      cy.nodes().remove();
      break;

    case 1:
    case 2:
    case 3:
      allNodes.restore();
      cy.filter("#AC").style({ "target-arrow-shape": "none" });
      cy.filter("#BC").style({ "target-arrow-shape": "none" });
      cy.filter("#CD").style({ "target-arrow-shape": "none" });
      cy.filter("#BD").style({ "target-arrow-shape": "none" });
      break;

    case 4:
      cy.filter("#AB").remove();
      cy.filter("#AD").remove();
      cy.filter("#AC").style({ "target-arrow-shape": "none" });
      cy.filter("#BC").style({ "target-arrow-shape": "none" });
      cy.filter("#CD").style({ "target-arrow-shape": "none" });
      cy.filter("#BD").style({ "target-arrow-shape": "none" });
      break;

    case 5:
      cy.filter("#AB").remove();
      cy.filter("#AD").remove();
      cy.filter("#AC").style({ "target-arrow-shape": "triangle" });
      cy.filter("#BC").style({ "target-arrow-shape": "triangle" });
      cy.filter("#CD").style({ "target-arrow-shape": "none" });
      cy.filter("#BD").style({ "target-arrow-shape": "none" });
      break;

    case 6:
      cy.filter("#AB").remove();
      cy.filter("#AD").remove();
      cy.filter("#AC").style({ "target-arrow-shape": "triangle" });
      cy.filter("#BC").style({ "target-arrow-shape": "triangle" });
      cy.filter("#CD").style({ "target-arrow-shape": "triangle" });
      cy.filter("#BD").style({ "target-arrow-shape": "none" });
      break;

    case 7:
      cy.filter("#AB").remove();
      cy.filter("#AD").remove();
      cy.filter("#AC").style({ "target-arrow-shape": "triangle" });
      cy.filter("#BC").style({ "target-arrow-shape": "triangle" });
      cy.filter("#CD").style({ "target-arrow-shape": "triangle" });
      cy.filter("#BD").style({ "target-arrow-shape": "triangle" });
      break;
  }
});
</script>

<template>
  <div class="w-full h-full" ref="viewer" />
</template>
