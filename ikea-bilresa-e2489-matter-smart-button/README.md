# IKEA BILRESA E2489 Matter Smart Button - Blueprint (+ Bounce Dimmer fork)

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fhubcasale%2Fhaos-blueprints%2Frefs%2Fheads%2Fmaster%2Fikea-bilresa-e2489-matter-smart-button%2Fikea-bilresa-e2489-matter-smart-button.yaml)

![BILRESA Matter Remote](https://github.com/censay/haos-blueprints/raw/master/ikea-bilresa-e2489-matter-smart-button/bilresa-remote.jpg)

This blueprint provides automation for controlling the [IKEA BILRESA E2489 Matter Smart Button](https://www.ikea.com/us/en/p/bilresa-remote-control-white-smart-dual-button-80617876/) in Home Assistant. 

Options for use (tested with lights and scripts):

- Turn on/off lights
- Control the brightness
- Initiate scenes
- Start scripts


## Fork addition: Bounce dimmer (v1.2.0)

This is a fork of [censay/haos-blueprints](https://github.com/censay/haos-blueprints) (MIT).
It adds an optional **bounce dimmer** to the long press of each button: pick a light in the
*Bounce dimmer* section and, **while you hold the button**, the brightness ramps **up to the maximum,
pauses, ramps down to the minimum, pauses, and repeats** until you release it. The light stays at the
level reached on release (the blueprint runs in `restart` mode, so the release event stops the loop).

All parameters are inputs: step (%), delay between steps (ms), minimum and maximum brightness,
pause at the ends, start level when the light is off, and a safety limit on the number of cycles.
It is **off** unless a light is selected, so existing automations keep working unchanged.
If the light is already at the maximum when you start holding, it begins by going down.
The long-press actions you configure (if any) run first, then the ramp starts.

## Installation (options)

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fhubcasale%2Fhaos-blueprints%2Frefs%2Fheads%2Fmaster%2Fikea-bilresa-e2489-matter-smart-button%2Fikea-bilresa-e2489-matter-smart-button.yaml)
- **OR** Search for "IKEA BILRESA" in Settings > Automation & Secenes > Blueprints (tab) > "Discover more blueprints"
- **OR** Manually copy [full yaml](#yaml) below to `/blueprints/automation/hubcasale/bilresa-remote.yaml`
- **OR** Import a blueprint from this address: [github.com/hubcasale/haos-blueprints](https://github.com/hubcasale/haos-blueprints/blob/master/ikea-bilresa-e2489-matter-smart-button/ikea-bilresa-e2489-matter-smart-button.yaml)

Restart Home Assistant or go to Devleoper Tools and check all yaml to reload your yaml files including the new blueprint.

## Usage

To use this blueprint, follow these steps:

1. Navigate to the "Automations" section in the Home Assistant UI.
2. Click on " + Create automation" in the corner button.
3. Define how you want the buttons to work.
4. Enjoy!

### Inputs

The following inputs are required for this blueprint:

- **Button Type**: Select the type of button press (single, double, long press, etc.).
- **Action**: Select the action to perform when the button is pressed (turn on, turn off, change brightness, etc.).
- **Entity**: Select the entity to control (e.g., switch, light, camera toggle).

## Requirements

This blueprint is built from ["Triggering the holidays" 2025.12](https://www.home-assistant.io/blog/2025/12/03/release-202512/) guidance so is designed to work best with...

- Home Assistant version 2025.12.1 or later.

## License

This blueprint is licensed under the [MIT License](https://opensource.org/licenses/MIT).

## Contributing

Contributions are welcome! If you have any suggestions, improvements, or bug fixes, please submit a pull request or open an issue on this repository.

## Credits

This blueprint was created by [censay](https://github.com/censay); the bounce dimmer was added in this fork by [hubcasale](https://github.com/hubcasale).

## Changelog
- **Version v1.2.0** (fork): Added the optional bounce dimmer (hold to ramp a light up and down) with configurable parameters
- **Version v1.1.0**: Added support for long press actions on press (long_press)
 - **Version 1.0.3**: Added [not_from/not_to](https://community.home-assistant.io/t/ikea-bilresa-matter-button-blueprint-for-ikea-matter-over-thread-button-actions-scripts-and-entities/962344/12) filters to prevent re-triggering on HA/Matter restarts ()
- **Version 1.0.2**: Initial release

## YAML
```
# ===================================================================
# IKEA BILRESA E2489 Dual Button + Bounce Dimmer (fork of censay/haos-blueprints)
# Blueprint Template v1.2.0
#
# Internal Versioning:
#   schema_version: 1.2.0
#   last_updated: 2026-11-15
#
# Changelog:
#   v1.2.0: (fork) Added the optional "Bounce dimmer": while a button is held, the
#           selected light ramps up and down until release. All parameters
#           (step, delay, limits, pauses, safety limit) are inputs.
#           Backward compatible: disabled unless a light is selected.
#   v1.1.0: Added support for long press actions on press (long_press)
#   v1.0.3: Added not_from/not_to filters to prevent re-triggering on HA/Matter restarts (https://community.home-assistant.io/t/ikea-bilresa-matter-button-blueprint-for-ikea-matter-over-thread-button-actions-scripts-and-entities/962344/12)
#   v1.0.2: Initial release
#
# Power-User Notes:
#   - This device exposes two event entities, one per physical button.
#   - You must select the correct event entity for Button 1 and Button 2.
#   - Each press updates the entity state (timestamp) and sets attributes.event_type.
# ===================================================================

blueprint:
  name: IKEA BILRESA E2489 Dual Button (Matter) + Bounce Dimmer
  author: censay (fork by hubcasale)
  description: >
    Full-featured automation for the IKEA BILRESA E2489 Matter dual-button
    remote. Supports single press, double press, and long press (on press and on release)
    for both buttons. Uses event entities exposed by Home Assistant for
    Matter devices. Single-press actions are prioritized for reliability.


    **Fork addition (v1.2.0): bounce dimmer.** Choose a light for a button in the
    "Bounce dimmer" section and, while you hold that button, the brightness ramps
    up to the maximum, pauses, ramps down to the minimum, pauses, and repeats until
    you release it. The light stays at the level reached on release. Step, speed,
    limits and pauses are configurable. Existing automations keep working
    unchanged: the bounce dimmer is off unless a light is selected.
  domain: automation
  source_url: https://github.com/hubcasale/haos-blueprints/blob/master/ikea-bilresa-e2489-matter-smart-button/ikea-bilresa-e2489-matter-smart-button.yaml
  homeassistant:
    min_version: 2025.12.1

  input:
    target_device:
      name: BILRESA Button Device
      description: >
        Select the IKEA BILRESA E2489 matter device.
      selector:
        device:
          filter:
            - manufacturer: IKEA of Sweden

    button1_event:
      name: Button 1 – Event Entity
      description: >
        Select the event entity for Button 1 (for example:
        event.bilresa_dual_button_button_1_2).
      selector:
        entity:
          domain: event

    button2_event:
      name: Button 2 – Event Entity
      description: >
        Select the event entity for Button 2 (for example:
        event.bilresa_dual_button_button_2_2).
      selector:
        entity:
          domain: event

    # ---------------------------------------------------------------
    # BUTTON 1 ACTIONS
    # ---------------------------------------------------------------

    button1_single:
      name: Button 1 – Single Press
      description: Action for Button 1 single press (multi_press_1).
      default: []
      selector:
        action: {}

    button1_double:
      name: Button 1 – Double Press
      description: Action for Button 1 double press (multi_press_2).
      default: []
      selector:
        action: {}

    button1_long_press:
      name: Button 1 – Long Press (on press)
      description: Action for Button 1 long press start (long_press).
      default: []
      selector:
        action: {}

    button1_long:
      name: Button 1 – Long Press (on release)
      description: Action for Button 1 long press completion (long_release).
      default: []
      selector:
        action: {}

    # ---------------------------------------------------------------
    # BUTTON 2 ACTIONS
    # ---------------------------------------------------------------

    button2_single:
      name: Button 2 – Single Press
      description: Action for Button 2 single press (multi_press_1).
      default: []
      selector:
        action: {}

    button2_double:
      name: Button 2 – Double Press
      description: Action for Button 2 double press (multi_press_2).
      default: []
      selector:
        action: {}

    button2_long_press:
      name: Button 2 – Long Press (on press)
      description: Action for Button 2 long press start (long_press).
      default: []
      selector:
        action: {}

    button2_long:
      name: Button 2 – Long Press (on release)
      description: Action for Button 2 long press completion (long_release).
      default: []
      selector:
        action: {}

    # ---------------------------------------------------------------
    # BOUNCE DIMMER (fork addition)
    # ---------------------------------------------------------------

    bounce_dimmer:
      name: Bounce dimmer (hold to ramp up and down)
      icon: mdi:lightbulb-on-50
      description: >
        While a button is held, the selected light ramps up and down until the
        button is released. Leave the light empty to disable it for that button.
        The long-press actions above (if any) run first, then the ramp starts.
      collapsed: false
      input:
        button1_ramp_light:
          name: Button 1 – Light to ramp
          description: Light dimmed up and down while Button 1 is held. Empty = disabled.
          default: ''
          selector:
            entity:
              domain: light
        button2_ramp_light:
          name: Button 2 – Light to ramp
          description: Light dimmed up and down while Button 2 is held. Empty = disabled.
          default: ''
          selector:
            entity:
              domain: light
        ramp_step_pct:
          name: Step (%)
          description: Brightness change at each step.
          default: 4
          selector:
            number:
              min: 1
              max: 20
              step: 1
              unit_of_measurement: '%'
              mode: slider
        ramp_step_delay_ms:
          name: Step delay (ms)
          description: Pause between two steps. Lower is faster.
          default: 120
          selector:
            number:
              min: 50
              max: 1000
              step: 10
              unit_of_measurement: ms
              mode: slider
        ramp_min_pct:
          name: Minimum brightness (%)
          description: The ramp turns around at this level and never turns the light off.
          default: 8
          selector:
            number:
              min: 1
              max: 50
              step: 1
              unit_of_measurement: '%'
              mode: slider
        ramp_max_pct:
          name: Maximum brightness (%)
          description: The ramp turns around at this level.
          default: 98
          selector:
            number:
              min: 50
              max: 100
              step: 1
              unit_of_measurement: '%'
              mode: slider
        ramp_edge_pause_ms:
          name: Pause at the ends (ms)
          description: Pause at the maximum and at the minimum before turning around.
          default: 400
          selector:
            number:
              min: 0
              max: 3000
              step: 50
              unit_of_measurement: ms
              mode: slider
        ramp_start_pct_when_off:
          name: Start level when the light is off (%)
          description: If the light is off, it is turned on at this level and then ramps up.
          default: 5
          selector:
            number:
              min: 1
              max: 50
              step: 1
              unit_of_measurement: '%'
              mode: slider
        ramp_max_cycles:
          name: Safety limit (up-and-down cycles)
          description: >
            The ramp stops by itself after this many full up-and-down cycles,
            even if the release event is never received.
          default: 30
          selector:
            number:
              min: 1
              max: 200
              step: 1
              mode: box

# ===================================================================
# AUTOMATION LOGIC
# ===================================================================

variables:
  button1_entity: !input button1_event
  button2_entity: !input button2_event
  ramp_light1: !input button1_ramp_light
  ramp_light2: !input button2_ramp_light
  ramp_step: !input ramp_step_pct
  ramp_delay: !input ramp_step_delay_ms
  ramp_min: !input ramp_min_pct
  ramp_max: !input ramp_max_pct
  ramp_pause: !input ramp_edge_pause_ms
  ramp_start: !input ramp_start_pct_when_off
  ramp_cycles: !input ramp_max_cycles

trigger:
  - platform: state
    id: button1
    entity_id: !input button1_event
    not_from: 'unavailable'
    not_to: 'unavailable'
  - platform: state
    id: button2
    entity_id: !input button2_event
    not_from: 'unavailable'
    not_to: 'unavailable'
  # Added not_from/not_to filters to prevent re-triggering on HA/Matter restarts; event entities use timestamp states

condition: []

action:
  - variables:
      press_type: "{{ trigger.to_state.attributes.event_type }}"
      trigger_id: "{{ trigger.id }}"

  - choose:

      # -------------------------------------------------------------
      # BUTTON 1
      # -------------------------------------------------------------

      - conditions:
          - condition: template
            value_template: >
              {{ trigger_id == 'button1' and press_type == 'multi_press_1' }}
        sequence: !input button1_single

      - conditions:
          - condition: template
            value_template: >
              {{ trigger_id == 'button1' and press_type == 'multi_press_2' }}
        sequence: !input button1_double

      - conditions:
        - condition: template
          value_template: >
            {{ trigger_id == 'button1' and press_type == 'long_press' }}
        sequence: !input button1_long_press

      - conditions:
          - condition: template
            value_template: >
              {{ trigger_id == 'button1' and press_type == 'long_release' }}
        sequence: !input button1_long

      # -------------------------------------------------------------
      # BUTTON 2
      # -------------------------------------------------------------

      - conditions:
          - condition: template
            value_template: >
              {{ trigger_id == 'button2' and press_type == 'multi_press_1' }}
        sequence: !input button2_single

      - conditions:
          - condition: template
            value_template: >
              {{ trigger_id == 'button2' and press_type == 'multi_press_2' }}
        sequence: !input button2_double

      - conditions:
        - condition: template
          value_template: >
            {{ trigger_id == 'button2' and press_type == 'long_press' }}
        sequence: !input button2_long_press

      - conditions:
          - condition: template
            value_template: >
              {{ trigger_id == 'button2' and press_type == 'long_release' }}
        sequence: !input button2_long

  # -----------------------------------------------------------------
  # BOUNCE DIMMER (fork addition)
  # Runs only on a long press of a button that has a light selected.
  # The blueprint runs in "restart" mode: the long_release event restarts it,
  # which stops the loops below immediately and leaves the light at the level
  # it reached.
  # -----------------------------------------------------------------
  - if:
      - condition: template
        value_template: >-
          {{ press_type == 'long_press' and
             (ramp_light1 if trigger_id == 'button1' else ramp_light2) not in ['', none] }}
    then:
      - variables:
          ramp_light: "{{ ramp_light1 if trigger_id == 'button1' else ramp_light2 }}"
          held_entity: "{{ button1_entity if trigger_id == 'button1' else button2_entity }}"
          max_b: "{{ (ramp_max | float(98) * 255 / 100) | round(0) | int }}"
          min_b: "{{ (ramp_min | float(8) * 255 / 100) | round(0) | int }}"
      - if:
          - condition: template
            value_template: "{{ is_state(ramp_light, 'off') }}"
        then:
          - action: light.turn_on
            target:
              entity_id: "{{ ramp_light }}"
            data:
              brightness_pct: "{{ ramp_start | int(5) }}"
      - repeat:
          while:
            - condition: template
              value_template: "{{ is_state_attr(held_entity, 'event_type', 'long_press') }}"
            - condition: template
              value_template: "{{ repeat.index <= (ramp_cycles | int(30)) }}"
          sequence:
            # ramp up
            - repeat:
                while:
                  - condition: template
                    value_template: "{{ is_state_attr(held_entity, 'event_type', 'long_press') }}"
                  - condition: template
                    value_template: "{{ state_attr(ramp_light, 'brightness') | int(0) < max_b }}"
                sequence:
                  - action: light.turn_on
                    target:
                      entity_id: "{{ ramp_light }}"
                    data:
                      brightness_step_pct: "{{ ramp_step | int(4) }}"
                  - delay:
                      milliseconds: "{{ ramp_delay | int(120) }}"
            - delay:
                milliseconds: "{{ ramp_pause | int(400) }}"
            # ramp down
            - repeat:
                while:
                  - condition: template
                    value_template: "{{ is_state_attr(held_entity, 'event_type', 'long_press') }}"
                  - condition: template
                    value_template: "{{ state_attr(ramp_light, 'brightness') | int(0) > min_b }}"
                sequence:
                  - action: light.turn_on
                    target:
                      entity_id: "{{ ramp_light }}"
                    data:
                      brightness_step_pct: "{{ 0 - (ramp_step | int(4)) }}"
                  - delay:
                      milliseconds: "{{ ramp_delay | int(120) }}"
            - delay:
                milliseconds: "{{ ramp_pause | int(400) }}"

mode: restart
```
