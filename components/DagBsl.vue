<script setup lang="ts">
import cytoscape from "cytoscape";
import { ref, watchEffect, nextTick } from "vue";
import { onSlideEnter, onSlideLeave, useSlideContext } from "@slidev/client";

const viewer = ref();
const { $clicks: clicks } = useSlideContext();

const score = defineModel();

let allNodes;
let AC;
let CD;

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
          "target-arrow-shape": "triangle",
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
  AC = cy.filter("#AC");
  CD = cy.filter("#CD");
});

onSlideLeave(() => {
  cyRef.value.destroy();
  cyRef.value = undefined;
  viewer.value.style.width = "100%";
  viewer.value.style.height = "100%";
});

let interv;
watchEffect(() => {
  if (!cyRef.value) return;
  const cy = cyRef.value;

  if (interv) {
    clearInterval(interv);
    interv = undefined;
  }

  switch (clicks.value) {
    case 0:
      cy.nodes().remove();
      break;

    case 1:
    case 2:
    case 3:
      allNodes.restore();
      cy.filter("#AC").remove();
      cy.filter("#CD").remove();
      cy.filter("#BD").style({
        "target-arrow-shape": "none",
        "source-arrow-shape": "triangle",
      });
      cy.filter("#AD").style({
        "target-arrow-shape": "triangle",
        "source-arrow-shape": "none",
        "source-arrow-color": "#ccc",
        "line-color": "#ccc",
      });
      score.value = 0.0024;
      break;
    case 4:
      cy.filter("#AD").style({
        "target-arrow-shape": "none",
        "source-arrow-shape": "triangle",
        "source-arrow-color": "#fa0",
        "line-color": "#fa0",
      });
      score.value = 0.0018;
      break;
    case 5:
    case 7:
      let evalStep = 0;
      const mutations = [
        // ------------------- REVERSE
        () =>
          cy.filter("#BD").style({
            "target-arrow-shape": "triangle",
            "source-arrow-shape": "none",
          }),
        () =>
          cy.filter("#BC").style({
            "target-arrow-shape": "none",
            "source-arrow-shape": "triangle",
          }),
        () =>
          cy.filter("#AB").style({
            "target-arrow-shape": "none",
            "source-arrow-shape": "triangle",
          }),
        // ------------------- ADD ARC
        () => AC.restore(),
        () => {
          CD.restore();
          CD.style({
            "target-arrow-shape": "none",
            "source-arrow-shape": "triangle",
          });
        },
        // ------------------- REMOVE ARC
        () => cy.filter("#AB").remove(),
        () => cy.filter("#BC").remove(),
        () => cy.filter("#AD").remove(),
        () => cy.filter("#BD").remove(),
        // ------------------- END
      ];
      interv = setInterval(
        () => {
          allNodes.restore();
          cy.filter("#AC").remove();
          cy.filter("#CD").remove();
          cy.edges().style({
            "target-arrow-shape": "triangle",
            "source-arrow-shape": "none",
            "source-arrow-color": "#ccc",
            "line-color": "#ccc",
          });
          cy.filter("#BD").style({
            "target-arrow-shape": "none",
            "source-arrow-shape": "triangle",
          });
          // -------------------
          mutations[evalStep++]();
          evalStep %= mutations.length;
          score.value = Math.random() / (clicks.value === 5 ? 100 : 10);
        },
        clicks.value === 5 ? 300 : 100,
      );
      break;
    case 6:
      allNodes.restore();
      cy.filter("#CD").remove();
      cy.filter("#BD").style({
        "target-arrow-shape": "none",
        "source-arrow-shape": "triangle",
      });
      cy.filter("#AD").style({
        "target-arrow-shape": "triangle",
        "source-arrow-shape": "none",
        "source-arrow-color": "#ccc",
        "line-color": "#ccc",
      });
      score.value = 0.0119;
      break;
    case 8:
      allNodes.restore();
      cy.filter("#AB").remove();
      cy.filter("#AD").remove();
      cy.edges().style({
        "target-arrow-shape": "triangle",
        "source-arrow-shape": "none",
        "source-arrow-color": "#ccc",
        "line-color": "#ccc",
      });
      score.value = 0.1873;
      break;
  }
});
</script>

<template>

  <div class="w-full h-full" ref="viewer" />
</template>
