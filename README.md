# BPMN 2.0 Extension for Custom Icons

This specification defines a BPMN 2.0 Extension for interchanging custom icons.

## Design considerations

The placement of the icons in the rendered diagram is not specified in this extension
and is deliberately left for to tool vendors to decide.

The icon is stored using the
[Data URI Scheme (RFC2397)](https://en.wikipedia.org/wiki/Data_URI_scheme)
because that is widely supported by many tools,
e.g. by SVG which is used by many tools for rendering BPMN.

SVG icons with no fill are prefered. For pixel graphics prefer transparent PNGs.

Since icons are relatively large, especially when not using vector graphics,
they should be reusable and placed in a central place in the XML serialization.

The extension should allow attaching custom icons to any BPMN element,
i.e. icons cannot be embedded in elements like `bpmn:Interface`
because only some of the BPMN elements use that element.

Unfortunately, neither the top-level `bpmn:definitions` not the `bpmndi:BPMNDiagram`
containers permit `extensionElements`. 
Therefore, this extension uses a top-level `bpmn:relationship` as a container, e.g.

```xml
  <relationship type="icons">
    <extensionElements>
      <icon:iconDefinition id="WebhookIcon" href="data:image/svg+xml,%3Csvg id=&#39;icon&#39; xmlns=&#39;http://www.w3.org/2000/svg&#39; width=&#39;18&#39; height=&#39;18&#39; viewBox=&#39;0 0 32 32&#39;%3E%3Cdefs%3E%3Cstyle%3E .cls-1 %7B fill: none; %7D %3C/style%3E%3C/defs%3E%3Cpath d=&#39;M24,26a3,3,0,1,0-2.8164-4H13v1a5,5,0,1,1-5-5V16a7,7,0,1,0,6.9287,8h6.2549A2.9914,2.9914,0,0,0,24,26Z&#39;/%3E%3Cpath d=&#39;M24,16a7.024,7.024,0,0,0-2.57.4873l-3.1656-5.5395a3.0469,3.0469,0,1,0-1.7326.9985l4.1189,7.2085.8686-.4976a5.0006,5.0006,0,1,1-1.851,6.8418L17.937,26.501A7.0005,7.0005,0,1,0,24,16Z&#39;/%3E%3Cpath d=&#39;M8.532,20.0537a3.03,3.03,0,1,0,1.7326.9985C11.74,18.47,13.86,14.7607,13.89,14.708l.4976-.8682-.8677-.497a5,5,0,1,1,6.812-1.8438l1.7315,1.002a7.0008,7.0008,0,1,0-10.3462,2.0356c-.457.7427-1.1021,1.8716-2.0737,3.5728Z&#39;/%3E%3Crect id=&#39;_Transparent_Rectangle_&#39; data-name=&#39;&#38;lt;Transparent Rectangle&#38;gt;&#39; class=&#39;cls-1&#39; width=&#39;32&#39; height=&#39;32&#39;/%3E%3C/svg%3E" />
    </extensionElements>
    <source>Definitions</source>
    <target>Definitions</target>
  </relationship>
```

The relationship's `source` and `target` MUST point to the `id` of the `bpmn:definitions` root element.

Icons are referenced from `BPMNDiagramElements` using `icon:iconRef`:

```xml
<bpmndi:BPMNShape id="StartEvent_di" bpmnElement="StartEvent" icon:iconRef="WebhookIcon">
```