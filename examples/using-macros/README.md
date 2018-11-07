# Example using global macros

This example uses a `macros.yaml` file to keep the included yaml files short and uniform.

## `lovelace/main.yaml`:

```yaml
name: Home
views:
  - !include bedroom.yaml
  - !include bathroom.yaml
  - !include kitchen.yaml
```

## `lovelace/macros.yaml`:

```yaml
{% macro light_profile(entity, profile, name, icon="lightbulb") -%}
{
  entity: light.{{ entity }},
  name: {{ name }},
  icon: "mdi:{{ icon }}",
  tap_action: call-service,
  service: light.turn_on,
  service_data: {
    entity_id: light.{{ entity }},
    profile: {{ profile }},
  }
}
{%- endmacro %}

{% macro light_glance(entity, name="Lamp") -%}
{
  type: glance,
  show_state: false,
  columns: 4,
  entities: [
    {
      entity: light.{{ entity }},
      name: {{ name }},
      tap_action: toggle
    },
    {{ light_profile(entity, "bright", "Bright", "lightbulb-on") }},
    {{ light_profile(entity, "normal", "Normal", "lightbulb-on-outline") }},
    {{ light_profile(entity, "night", "Night", "lightbulb-outline") }},
  ]
}
{%- endmacro %}
```


## `lovelace/bedroom.yaml`:

```yaml
name: Bedroom
title: Bedroom
id: bedroom
icon: mdi:bed-empty
cards:
  - {{ light_glance("bedroom_lamp") }}
```


## `lovelace/bathroom.yaml`:

Mostly the same as the last room.

```yaml
name: Bathroom
title: Bathroom
id: bathroom
icon: mdi:bath-empty
cards:
  - {{ light_glance("bathroom_lamp") }}
```


## `lovelace/bathroom.yaml`:

This room is slighty adapted for two lights.

```yaml
name: "Kitchen"
title: "Kitchen"
id: kitchen
icon: mdi:silverware-variant
cards:
  - type: glance
    show_state: false
    columns: 4
    entities:
      - entity: light.kitchen_leds
        icon: mdi:led-outline
        name: LEDs
        tap_action: toggle
      - entity: light.kitchen_lamp
        name: Lamp
        tap_action: toggle
      - {{ light_profile("kitchen_lamp", "bright", "Bright", "lightbulb-on") }}
      - {{ light_profile("kitchen_lamp", "night", "Night", "lightbulb-outline") }}
```
