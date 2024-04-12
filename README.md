# Giao diện mobile cho HomeAssistant
# Sau đây là code, cách nhanh nhất để sử dụng là copy toàn bộ rồi paste,xong báo lỗi rồi chỉnh sửa từng cái.

decluttering_templates:
  header_temperature_graph:
    card:
      type: custom:mini-graph-card
      entities:
        - entity: sensor.aqara_temperature_and_humidity_sensor_temperature # cần thay thế
          color: null
      hours_to_show: 24
      hour24: true
      animate: true
      update_interval: 30
      aggregate_func: avg
      line_width: 1
      bar_spacing: 10
      height: 40
      group: true
      show:
        labels: false
        fill: false
        state: false
        name: false
        icon: false
      style: |
        ha-card {
          height: 84px;
          position: absolute;
          box-shadow: none;
          background: none;
          border-radius: 0;
          opacity: 0.3;
          margin-bottom: -90px;
          margin-left: -0.6em;
          margin-right: -0.6em;
        }
  header_main:
    card:
      type: custom:paper-buttons-row
      styles:
        justify-content: space-between
        background: none
        margin: 0px 0px 0px
        text-shadow: 0px 0px 0px var(--card-background-color);
      buttons:
        - layout: name_state
          name: '{{ states(''sensor.date'') }}, {{ states(''sensor.month'') }}'        # cần tạo thêm sensor date, month
          state: '{{ now().strftime(''%H'') }}:{{ now().strftime(''%M'')}}'
          styles:
            name:
              font-weight: 400
              font-size: 16px
              margin-left: 20px
              display: block
              text-align: left
              float: left
              width: 170px
            button:
              pointer-events: none
              align-items: left
              width: 140px
              line-height: 1.6em
            state:
              font-weight: 700
              font-size: 30px
              letter-spacing: '-1px'
              margin-left: 0
              display: block
              text-align: left
              float: left
              width: 140px
        - layout: icon|name_state
          name: >-
            ⌂ {{
            states('sensor.aqara_temperature_and_humidity_sensor_temperature') # cần thay thế
            }}°C
          state: '{{ state_attr(''weather.forecast_home'',''temperature'') }}°C'  # cần thay thế
          image: |
            {% set mapper =
              { 'breezy':'cloudy',
                'clear-night':'night',
                'clear':'day',
                'mostly-clear':'day',
                'clear-day':'day',
                'cloudy':'cloudy',
                'fog':'fog',
                'hail':'rainy-7',
                'haze':'haze',
                'lightning':'thunder',
                'mostly-cloudy':'cloudy',
                'partlycloudy':'cloudy-day-3',
                'partly-cloudy-day':'partly-cloudy-day',
                'partly-cloudy-night':'partly-cloudy-night',
                'rainy':'rainy-4',
                'scattered-showers':'rainy-3',
                'showers':'rainy-6',
                'sleet':'sleet',
                'snow':'snowy-6',
                'mostly-sunny':'day',
                'sunny':'day',
                'thunderstorm':'thunder',
                'tornado':'tornado',
                'wind':'wind',
                'windy':'wind'} %}
            {% set state = states('weather.forecast_home') %}     # cần thay thế
            {% set weather = mapper[state] if state in mapper else 'weather' %}
            {% set path = '/local/icon/weather_icon/animated/' %}
            {% set ext = '.svg'%}
              {{[path,weather,ext]|join('')}}
          styles:
            name:
              font-weight: 400
              font-size: 16px
              margin-left: 0
              display: block
              text-align: right
              width: 140px
            button:
              pointer-events: none
              align-items: right
              width: 140px
              line-height: 1.6em
            state:
              font-weight: 600
              font-size: 30px
              letter-spacing: '-1px'
              margin-left: 0
              display: block
              text-align: right
              width: 140px
            icon:
              position: absolute
              left: 2px
              transform: scale(0.17)
              transform-origin: 0 19.5%
              top: '-48px;'
              padding: 0;
              height: 380px !important;
              width: 380px !important;
  footer_sticky_menu:
    card:
      type: entities
      entities:
        - type: custom:paper-buttons-row
          buttons:
            - layout: icon
              icon: phu:homekit # cài thêm bộ icon phụ
              name: Home
              tap_action:
                action: navigate
                navigation_path: /dashboard-a/home
              style:
                button:
                  width: 42px
                  height: 42px
                icon:
                  margin-top: 0
                name:
                  margin-top: 20px
            - layout: icon
              icon: hue:room-storage
              name: Room
              tap_action:
                action: navigate
                navigation_path: /dashboard-a/room
              style:
                button:
                  width: 42px
                  height: 42px
                icon:
                  margin-top: 0
                name:
                  margin-top: 20px
            - layout: icon
              icon: mdi:ansible
              name: Auto
              tap_action:
                action: navigate
                navigation_path: /dashboard-a/auto
              style:
                button:
                  width: 42px
                  height: 42px
                icon:
                  margin-top: 0
                name:
                  margin-top: 20px
            - layout: icon
              icon: mdi:cog
              name: System
              tap_action:
                action: navigate
                navigation_path: /dashboard-a/setting
              style:
                button:
                  width: 42px
                  height: 42px
                icon:
                  margin-top: 0
                name:
                  margin-top: 20px
            - layout: icon
              icon: mdi:server-minus
              name: info
              tap_action:
                action: navigate
                navigation_path: /dashboard-a/info
              style:
                button:
                  width: 42px
                  height: 42px
                icon:
                  margin-top: 0
                name:
                  margin-top: 20px
          style: |
            div.flex-box {
              display: flex;
              justify-content: space-between;
              align-items: center;
              margin: -14px !important;   
            }
      style: |
        ha-card { 
          margin-bottom: 10px !important;
          padding: 0 16px;
          border-radius: 34px !important;
          margin-left: 0px;
          margin-right: 0px;
        }
title: ''
views:
  - theme: noctis
    icon: phu:homekit
    path: home
    title: HOME
    type: sidebar
    badges: []
    cards:
      - type: custom:decluttering-card
        template: header_temperature_graph
      - type: custom:decluttering-card
        template: header_main
      - type: custom:vertical-stack-in-card
        cards:
          - type: horizontal-stack
            cards:
              - type: custom:slider-button-card
                entity: input_boolean.home
                name: Home
                slider:
                  direction: left-right
                  background: gradient
                  use_state_color: true
                  use_percentage_bg_opacity: true
                  show_track: false
                  toggle_on_click: true
                  force_square: false
                show_name: true
                show_state: false
                compact: true
                icon:
                  show: true
                  use_state_color: false
                  icon: mdi:home-import-outline
                  tap_action:
                    service: input_boolean.turn_on
                    data: {}
                    target:
                      entity_id: input_boolean.home
                action_button:
                  mode: custom
                  icon: mdi:chevron-right
                  show_spinner: false
                  tap_action:
                    action: navigate
                    navigation_path: /config/scene/edit/1689999241266
                style: |
                  :host {
                    margin-right: 7px !important;
                  }
                  ha-card > div > div.text {
                    font-weight: 600;
                    font-size: 14px;
                    margin-top: -1px;
                    letter-spacing: -0.2px;
                  }
                  div.icon {
                    margin: 4px 2px 4px 4px;
                    font-size: 10px;
                  }
                  ha-card > div > div.text > div {
                    overflow: visible;
                  }
                  ha-card { 
                    border-radius: 26px !important;
                  }
                  ha-card > div > div.action {
                    position: absolute;
                    top: 13px;
                    right: 6px;
                    font-size: 12px;
                  }
              - type: custom:slider-button-card
                entity: input_boolean.leave
                name: Leave
                slider:
                  direction: left-right
                  background: gradient
                  use_state_color: true
                  use_percentage_bg_opacity: true
                  show_track: false
                  toggle_on_click: true
                  force_square: false
                show_name: true
                show_state: false
                compact: true
                icon:
                  show: true
                  use_state_color: false
                  icon: mdi:home-export-outline
                  tap_action: null
                  service: input_boolean.turn_on
                  data: {}
                  target:
                    entity_id: input_boolean.leave
                action_button:
                  mode: custom
                  icon: mdi:chevron-right
                  show_spinner: false
                  tap_action:
                    action: navigate
                    navigation_path: /config/scene/edit/1690024250875
                style: |
                  :host {
                    font-weight: 600;
                    margin-left: 7px !important;
                  }
                  ha-card > div > div.text {
                    font-weight: 600;
                    font-size: 14px;
                    margin-top: -1px;
                    letter-spacing: -0.2px;
                  }
                  div.icon {
                    margin: 4px 2px 4px 4px;
                    font-size: 10px;
                  }
                  ha-card > div > div.text > div {
                    overflow: visible;
                  }
                  ha-card { 
                    border-radius: 26px !important;
                  }
                  ha-card > div > div.action {
                    position: absolute;
                    top: 13px;
                    right: 6px;
                    font-size: 12px;
                  }
        style: |
          ha-card { 
            margin: 0px 0px !important;
            background: none;
          }
      - type: custom:vertical-stack-in-card
        cards:
          - type: horizontal-stack
            cards:
              - type: custom:slider-button-card
                entity: input_boolean.a
                name: Morning
                slider:
                  direction: left-right
                  background: gradient
                  use_state_color: true
                  use_percentage_bg_opacity: true
                  show_track: false
                  toggle_on_click: true
                  force_square: false
                show_name: true
                show_state: false
                compact: true
                icon:
                  show: true
                  use_state_color: false
                  icon: mdi:white-balance-sunny
                  tap_action: null
                  service: input_boolean.turn_on
                  data: {}
                  target:
                    entity_id: input_boolean.a
                action_button:
                  mode: custom
                  icon: mdi:chevron-right
                  show_spinner: false
                  tap_action:
                    action: navigate
                    navigation_path: /config/scene/edit/1690024313694
                style: |
                  :host {
                    font-weight: 600;
                    margin-right: 7px !important;
                  }
                  ha-card > div > div.text {
                    font-weight: 600;
                    font-size: 14px;
                    margin-top: -1px;
                    letter-spacing: -0.2px;
                  }
                  div.icon {
                    margin: 4px 2px 4px 4px;
                    font-size: 10px;
                  }
                  ha-card > div > div.text > div {
                    overflow: visible;
                  }
                  ha-card { 
                    border-radius: 26px !important;
                  }
                  ha-card > div > div.action {
                    position: absolute;
                    top: 13px;
                    right: 6px;
                    font-size: 12px;
                  }
              - type: custom:slider-button-card
                entity: input_boolean.sleep
                name: Sleep
                slider:
                  direction: left-right
                  background: gradient
                  use_state_color: false
                  use_percentage_bg_opacity: true
                  show_track: false
                  toggle_on_click: true
                  force_square: false
                show_name: true
                show_state: false
                compact: true
                icon:
                  show: true
                  use_state_color: false
                  icon: mdi:power-sleep
                  tap_action: null
                  service: input_boolean.turn_on
                  data: {}
                  target:
                    entity_id: input_boolean.sleep
                action_button:
                  mode: custom
                  icon: mdi:chevron-right
                  show_spinner: false
                  tap_action:
                    action: navigate
                    navigation_path: /config/scene/edit/1690024336037
                style: |
                  :host {
                    font-weight: 600;
                    margin-left: 7px !important;
                  }
                  ha-card > div > div.text {
                    font-weight: 600;
                    font-size: 14px;
                    margin-top: -1px;
                    letter-spacing: -0.2px;
                  }
                  div.icon {
                    margin: 4px 2px 4px 4px;
                    font-size: 10px;
                  }
                  ha-card > div > div.text > div {
                    overflow: visible;
                  }
                  ha-card { 
                    border-radius: 26px !important;

                  }
                  ha-card > div > div.action {
                    position: absolute;
                    top: 13px;
                    right: 6px;
                    font-size: 12px;
                  }
        style: |
          ha-card { 
            margin: 0px 0px !important;
            background: none;
          }
      - type: custom:vertical-stack-in-card
        cards:
          - type: horizontal-stack
            cards:
              - type: grid
                square: true
                columns: 2
                cards:
                  - type: custom:vertical-stack-in-card
                    cards:
                      - type: custom:weather-card
                        entity: weather.forecast_home
                        icons: /local/icon/weather_icon/animated/
                        number_of_forecasts: '3'
                        current: false
                        details: false
                        forecast: true
                        hourly_forecast: false
                        tap_action:
                          action: none
                        style: |
                          ha-card { 
                            background: none;
                            box-shadow: none;
                            padding: 0 !important;
                            font-size: 11px;
                          }
                          div.precipitation {
                            display: none;
                           }
                          div.precipitation_probability {
                            display: none;
                          }
                          i.icon {
                            transform: scale(0.17);
                            transform-origin: 0 19.5%;
                            margin-left: -1.4em !important;
                            position: absolute;
                              top: -18px;
                              padding: 0;
                              height: 240% !important;
                              width: 240% !important;
                          }
                          .dayname:after {
                           content:'';
                           height: 3em;
                           width: 3em;
                           display: block;
                           margin-top: -10px;
                          }
                          .dayname {
                            margin-top: -6px
                          }
                          div.day {
                            border-right: 0.06em solid rgb(26,137,245,0.3);
                          }
                          div.lowTemp {
                            margin-top: -10px;
                          }
                    style: |
                      ha-card { 
                      box-shadow: none;
                      padding: 13% 2%;
                      height: 68%
                  - type: horizontal-stack
                    cards:
                      - type: custom:mushroom-template-card
                        primary: Man 2
                        secondary: ' {{states(''sensor.iphone_12_mini_battery_level'')}}%'
                        icon: mdi:home
                        picture: /local/a.jpg
                        entity: person.man_1
                        layout: vertical
                        badge_icon: |-
                          {% if is_state(entity, 'home') %}
                          mdi:home
                          {% elif is_state(entity, 'away') %}
                          mdi:home-export-outline
                          {% else %}
                          mdi:navigation-variant
                          {% endif %}
                        badge_color: |-
                          {% if is_state(entity, 'home') %}
                          green
                          {% elif is_state(entity, 'away') %}
                          orange
                          {% else %}
                          blue
                          {% endif %}
                        fill_container: false
                        tap_action:
                          action: navigate
                          navigation_path: /dashboard-a/person_1
                      - type: custom:mushroom-template-card
                        primary: Man 1
                        secondary: ' {{states(''sensor.iphone_7_battery_level'')}}%'
                        icon: mdi:home
                        picture: /local/b.png
                        entity: person.man_1
                        layout: vertical
                        badge_icon: |-
                          {% if is_state(entity, 'home') %}
                          mdi:home
                          {% elif is_state(entity, 'away') %}
                          mdi:home-export-outline
                          {% else %}
                          mdi:navigation-variant
                          {% endif %}
                        badge_color: |-
                          {% if is_state(entity, 'home') %}
                          green
                          {% elif is_state(entity, 'away') %}
                          orange
                          {% else %}
                          blue
                          {% endif %}
                        fill_container: false
        style: |
          ha-card { 
            margin: 0px 0px !important;
            background: none;
            box-shadow: none;
            height: 120px;

          }
          #states > :last-child {
          z-index: 10;
          }
      - type: custom:vertical-stack-in-card
        cards:
          - type: horizontal-stack
            cards:
              - type: grid
                square: true
                columns: 2
                cards:
                  - type: custom:mushroom-light-card
                    entity: light.master_light
                    name: Master
                    icon: phu:ceiling-round
                    fill_container: false
                    show_brightness_control: true
                    show_color_temp_control: true
                    use_light_color: false
                    icon_color: orange
                    collapsible_controls: false
                    tap_action:
                      action: none
                    hold_action:
                      action: toggle
                  - type: custom:mushroom-humidifier-card
                    entity: humidifier.dmaker_22l_c3a8_dehumidifier
                    name: Dehumidifier
                    icon_color: blue
                    fill_container: false
                    show_target_humidity_control: true
                    tap_action:
                      action: none
        style: |
          ha-card { 
            margin: 0px 0px !important;
            background: none;
            box-shadow: none;
            min-width: auto;
            max-width: auto;
            height: 120px
          }
          #states > :last-child {
          z-index: 10;
          }
      - type: custom:vertical-stack-in-card
        cards:
          - type: horizontal-stack
            cards:
              - type: grid
                square: true
                columns: 2
                cards:
                  - type: custom:mushroom-climate-card
                    entity: climate.pm
                    icon: phu:air-conditioner
                    layout: null
                    fill_container: false
                    hvac_modes:
                      - cool
                      - 'off'
                    tap_action:
                      action: navigate
                      navigation_path: /dashboard-a/ac
                    show_temperature_control: true
                  - type: custom:mushroom-cover-card
                    entity: cover.aqara_curtain_controller
                    name: Curtain
                    layout: null
                    fill_container: false
                    show_position_control: false
                    show_tilt_position_control: false
                    show_buttons_control: true
                    tap_action:
                      action: none
        style: |
          ha-card { 
            margin: 0px 0px !important;
            background: none;
            box-shadow: none;
            height: 120px;important;
          }

          #states > :last-child {
            z-index: 10;
          }
      - type: custom:vertical-stack-in-card
        cards:
          - type: horizontal-stack
            cards:
              - type: grid
                square: true
                columns: 2
                cards:
                  - type: custom:mushroom-media-player-card
                    entity: media_player.tv
                    icon: phu:apple-tv
                    fill_container: false
                    volume_controls: []
                    media_controls:
                      - on_off
                      - previous
                      - play_pause_stop
                      - next
                    tap_action:
                      action: navigate
                      navigation_path: /dashboard-a/tv
                  - type: custom:mushroom-media-player-card
                    entity: media_player.mt_homepod
                    icon: phu:homepod-mini
                    name: Hompod
                    volume_controls:
                      - volume_buttons
                    media_controls:
                      - on_off
                      - previous
                      - play_pause_stop
                      - next
                    use_media_info: true
                    show_volume_level: true
                    fill_container: false
                    tap_action:
                      action: none
        style: |
          ha-card { 
            margin: 0px 0px !important;
            background: none;
            box-shadow: none;
            height: 120px;
          }

          #states > :last-child {
            z-index: 10;
          }
      - type: custom:vertical-stack-in-card
        cards:
          - type: horizontal-stack
            cards:
              - type: grid
                square: true
                columns: 2
                cards:
                  - type: custom:mushroom-vacuum-card
                    entity: vacuum.robot_don_nha
                    icon: phu:roborock
                    icon_animation: true
                    commands:
                      - start_pause
                      - stop
                      - return_home
                    name: Sen
                    layout: null
                    fill_container: false
                    tap_action:
                      action: navigate
                      navigation_path: /dashboard-a/roborock
                  - type: custom:mushroom-fan-card
                    entity: fan.dmaker_02_2af0_fan_2
                    icon_animation: true
                    show_percentage_control: true
                    show_oscillate_control: true
                    collapsible_controls: false
                    fill_container: false
                    name: Master
                    tap_action:
                      action: navigate
                      navigation_path: /dashboard-a/fan
        style: |
          ha-card { 
            margin: 0px 0px !important;
            background: none;
            box-shadow: none;
            height: 120px;
          }

          #states > :last-child {
            z-index: 10;
          }
      - type: custom:vertical-stack-in-card
        cards:
          - type: custom:decluttering-card
            template: footer_sticky_menu
        style: |
          :host {
            position: sticky !important;
            bottom: 26px;
            margin-bottom: 10px !important;
            animation: 1.2s position ease-in-out;
          }
          @keyframes position {
            0% { bottom: -80px; }
            20% { bottom: -80px; }
            70% { bottom: 26px; }
            90% { bottom: 24px; }
            100% { bottom: 26px; }
          }
          ha-card { 
            background: none;
            border-radius: 26px !important;
          }
          :host:before {
            content: '';
            display: block;
            position: absolute;
            bottom: -26px;
            left: -8px;
            padding-right: 16px;
            height: 130px;
            width: 100%; 
            background: linear-gradient(180deg, rgba(45, 56, 76, 0) 0%, rgba(35, 46, 66, 0.85) 50%);
            pointer-events: none;
            animation: 1.2s opacity ease-in-out;
          }
          @keyframes opacity {
            0% { opacity: 0; }
            20% { opacity: 0; }
            100% { opacity: 1; }
          }
          ### The sticky position don't work with Decluttering card
          ### This is why I have to add the CSS here
          ### If you don't use the UI you can use YAML anchors instead
  - theme: noctis
    icon: mdi:devices
    path: setting
    title: Setting
    type: sidebar
    subview: true
    badges: []
    cards:
      - type: custom:decluttering-card
        template: header_temperature_graph
      - type: custom:decluttering-card
        template: header_main
      - type: entities
        entities:
          - type: custom:paper-buttons-row
            buttons:
              - icon: mdi:cog
                layout: icon|name
                name: Coming Soon
                style:
                  name:
                    margin-left: 18px
                    font-size: 18px
                    font-weight: 600
            style: |
              div.flex-box {
                margin-top: -6px;
                display: flex;
                justify-content: space-between;
                align-items: center;
              }
          - type: section
        style: |
          ha-card {
            margin: 0px 0px 0px !important;
          }
      - type: custom:vertical-stack-in-card
        cards:
          - type: custom:decluttering-card
            template: footer_sticky_menu
        style: |
          :host {
            position: sticky !important;
            bottom: 26px;
            margin-bottom: 0px !important;
            animation: 1.2s position ease-in-out;
          }
          @keyframes position {
            0% { bottom: -80px; }
            20% { bottom: -80px; }
            70% { bottom: 26px; }
            90% { bottom: 24px; }
            100% { bottom: 26px; }
          }
          ha-card { 
            background: none;
            border-radius: 26px !important;
            margin-top: 50%;
          }
          :host:before {
            content: '';
            display: block;
            position: absolute;
            bottom: -26px;
            left: -8px;
            padding-right: 16px;
            height: 130px;
            width: 100%; 
            background: linear-gradient(180deg, rgba(45, 56, 76, 0) 0%, rgba(35, 46, 66, 0.85) 50%);
            pointer-events: none;
            animation: 1.2s opacity ease-in-out;
          }
          @keyframes opacity {
            0% { opacity: 0; }
            20% { opacity: 0; }
            100% { opacity: 1; }
          }
          ### The sticky position don't work with Decluttering card
          ### This is why I have to add the CSS here
          ### If you don't use the UI you can use YAML anchors instead
  - theme: noctis
    icon: phu:rooms-storage
    path: room
    title: room
    visible:
      - user: a27a825440914ad7a919367c2e53793e
      - user: aadd52d881af46bca495e279bee867bf
      - user: 10a78a75e63f4c5ba22a81742f59b925
    type: sidebar
    subview: true
    badges: []
    cards:
      - type: custom:decluttering-card
        template: header_temperature_graph
      - type: custom:decluttering-card
        template: header_main
      - type: custom:vertical-stack-in-card
        cards:
          - type: custom:mushroom-template-card
            primary: Living Room
            secondary: >-
              {{
              states('sensor.cleargrass_dk1_ae69_temperature_humidity_sensor')}}°C
              - {{ states('sensor.cleargrass_dk1_ae69_relative_humidity')}}%
            tap_action:
              action: navigate
              navigation_path: /dashboard-a/living
            card_mod:
              style: |
                ha-card {
                  background: transparent;
                }     
          - type: custom:mushroom-chips-card
            chips:
              - type: entity
                entity: fan.server_fan
                tap_action:
                  action: toggle
                icon_color: green
                icon: mdi:fan
              - type: entity
                entity: media_player.hanh_lang
                tap_action:
                  action: toggle
                icon_color: purple
                icon: phu:home-mini
            card_mod:
              style: |
                ha-card {
                  --chip-background: rgba(var(--rgb-red), 0.0);
                  margin-top: 40px !important;
                }
            alignment: end
        card_mod:
          style: |
            ha-card  {
              background: url('/local/livingd.jpg') center;
              background-size: cover;
              --mush-icon-size: 76px;
              --primary-text-color: white;
              height: 140px;
             --secondary-text-color: white ;
            }
      - type: custom:vertical-stack-in-card
        cards:
          - type: custom:mushroom-template-card
            primary: Master
            secondary: >-
              {{
              states('sensor.aqara_temperature_and_humidity_sensor_temperature')|round(1)}}°C
              - {{
              states('sensor.aqara_temperature_and_humidity_sensor_humidity')|round(1)}}%
            tap_action:
              action: navigate
              navigation_path: /dashboard-a/master
            card_mod:
              style: |
                ha-card {
                  background: transparent;
                } 
          - type: custom:mushroom-chips-card
            chips:
              - type: entity
                entity: binary_sensor.aqara_door_and_window_sensor_door
                icon_color: orange
                icon: mdi:door
              - type: entity
                entity: binary_sensor.aqara_motion_sensor_p1_occupancy
                icon_color: yellow
                icon: mdi:run
              - type: entity
                entity: light.master_light
                icon_color: yellow
                icon: mdi:lightbulb
              - type: entity
                entity: climate.pm
                icon_color: blue
                icon: phu:air-conditioner
            card_mod:
              style: |
                ha-card {
                  --chip-background: rgba(var(--rgb-red), 0.0);
                  margin-top: 40px !important;
                }
            alignment: end
        card_mod:
          style: |
            ha-card  {
              background: url('/local/bedroomm.jpg') center;
              background-size: cover;
              --mush-icon-size: 76px;
              --primary-text-color: white;
              height: 140px;
             --secondary-text-color:  ;
            }   
      - type: custom:vertical-stack-in-card
        cards:
          - type: custom:mushroom-template-card
            primary: AC Room
            secondary: >-
              {{
              states('sensor.cleargrass_dk1_b2a6_temperature_humidity_sensor')}}°C
              - {{ states('sensor.cleargrass_dk1_b2a6_relative_humidity')}}%
            tap_action:
              action: navigate
              navigation_path: /dashboard-a/ac2
            card_mod:
              style: |
                ha-card {
                  background: transparent;
                } 
          - type: custom:mushroom-chips-card
            chips:
              - type: entity
                entity: sensor.illumination_04cf8c9151ce
                icon_color: yellow
                icon: mdi:weather-sunny
              - type: entity
                entity: light.gateway_light_04cf8c9151ce
                icon_color: orange
                icon: mdi:light-recessed
              - type: entity
                entity: media_player.md_homepod
                icon_color: blue
                icon: phu:homepod-mini
            card_mod:
              style: |
                ha-card {
                  --chip-background: rgba(var(--rgb-red), 0.0);
                  margin-top: 40px !important;
                }
            alignment: end
        card_mod:
          style: |
            ha-card  {
              background: url('/local/bedroomk.jpg') center;
              background-size: cover;
              background-color: blur;
              --mush-icon-size: 76px;
              --primary-text-color: white;
              height: 140px;
              margin-left: 0px !important;
             --secondary-text-color:  ;
            }   
      - type: custom:vertical-stack-in-card
        cards:
          - type: custom:mushroom-template-card
            primary: Mom
            secondary: >-
              {{
              states('sensor.cleargrass_dk1_ae69_temperature_humidity_sensor')}}°C
              - {{ states('sensor.cleargrass_dk1_ae69_relative_humidity')}}%
            tap_action:
              action: navigate
              navigation_path: /dashboard-a/mom
            card_mod:
              style: |
                ha-card {
                  background: transparent;
                }       
          - type: custom:mushroom-chips-card
            chips:
              - type: entity
                entity: light.gateway_light_04cf8ca07ea5
                icon_color: orange
                icon: mdi:light-recessed
              - type: entity
                entity: climate.ac
                icon_color: blue
                icon: phu:air-conditioner
            card_mod:
              style: |
                ha-card {
                  --chip-background: rgba(var(--rgb-red), 0.0);
                  margin-top: 40px !important;
                }
            alignment: end
        card_mod:
          style: |
            ha-card  {
              background: url('/local/bedroome.jpg') center;
              background-size: cover;
              background-color: blur;
              --mush-icon-size: 76px;
              --primary-text-color: white;
              height: 140px;
              margin-left: 0px !important;
             --secondary-text-color:  ;
            }   
      - type: custom:vertical-stack-in-card
        cards:
          - type: custom:decluttering-card
            template: footer_sticky_menu
        style: |
          :host {
            position: sticky !important;
            bottom: 26px;
            margin-bottom: 10px !important;
            animation: 1.2s position ease-in-out;
          }
          @keyframes position {
            0% { bottom: -80px; }
            20% { bottom: -80px; }
            70% { bottom: 26px; }
            90% { bottom: 24px; }
            100% { bottom: 26px; }
          }
          ha-card { 
            background: none;
            border-radius: 26px !important;
          }
          :host:before {
            content: '';
            display: block;
            position: absolute;
            bottom: -26px;
            left: -8px;
            padding-right: 16px;
            height: 130px;
            width: 100%; 
            background: linear-gradient(180deg, rgba(45, 56, 76, 0) 0%, rgba(35, 46, 66, 0.85) 50%);
            pointer-events: none;
            animation: 1.2s opacity ease-in-out;
          }
          @keyframes opacity {
            0% { opacity: 0; }
            20% { opacity: 0; }
            100% { opacity: 1; }
          }
          ### The sticky position don't work with Decluttering card
          ### This is why I have to add the CSS here
          ### If you don't use the UI you can use YAML anchors instead
  - theme: noctis
    icon: phu:desktop-computer
    path: info
    title: Info
    visible:
      - user: a27a825440914ad7a919367c2e53793e
      - user: aadd52d881af46bca495e279bee867bf
      - user: 10a78a75e63f4c5ba22a81742f59b925
    type: sidebar
    subview: true
    badges: []
    cards:
      - type: custom:decluttering-card
        template: header_temperature_graph
      - type: custom:decluttering-card
        template: header_main
      - type: entities
        entities:
          - type: custom:paper-buttons-row
            buttons:
              - icon: phu:rooms-computer
                layout: icon|name
                name: Server
                style:
                  name:
                    margin-left: 18px
                    font-size: 18px
                    font-weight: 600
            style: |
              div.flex-box {
                margin-top: -6px;
                display: flex;
                justify-content: space-between;
                align-items: center;
              }            
          - type: section
          - type: custom:stack-in-card
            cards:
              - type: horizontal-stack
                cards:
                  - type: custom:mushroom-entity-card
                    entity: sensor.local_ip
                    tap_action: null
                    action: toggle
                    icon_color: null
                  - type: custom:mushroom-entity-card
                    entity: sensor.speedtest_ping
                    name: Ping
                    icon: mdi:clock
                    tap_action: null
                    action: toggle
                    icon_color: yellow
          - type: custom:stack-in-card
            cards:
              - type: horizontal-stack
                cards:
                  - type: custom:mushroom-entity-card
                    entity: sensor.speedtest_download
                    tap_action: null
                    action: toggle
                    icon_color: green
                    icon: mdi:download
                    name: Download
                  - type: custom:mushroom-entity-card
                    entity: sensor.speedtest_upload
                    icon: mdi:upload
                    icon_color: blue
                    name: Upload
                    tap_action: null
                    action: toggle
          - type: section
          - type: custom:stack-in-card
            cards:
              - type: horizontal-stack
                cards:
                  - type: custom:mushroom-entity-card
                    entity: sensor.cpu_speed
                    icon_color: red
                  - type: custom:mushroom-entity-card
                    entity: sensor.cpu_temperature
                    name: CPU Temp
                    icon_color: orange
          - type: custom:stack-in-card
            cards:
              - type: horizontal-stack
                cards:
                  - type: custom:mushroom-entity-card
                    entity: sensor.memory_free
                    icon_color: green
                  - type: custom:mushroom-entity-card
                    entity: sensor.memory_use
                    icon_color: green
          - type: custom:stack-in-card
            cards:
              - type: horizontal-stack
                cards:
                  - type: custom:mushroom-entity-card
                    entity: sensor.disk_free
                    name: Disk Free
                    icon_color: purple
                  - type: custom:mushroom-entity-card
                    entity: sensor.disk_use
                    name: Disk Use
            icon_color: brown
          - type: section
          - type: custom:stack-in-card
            cards:
              - type: horizontal-stack
                cards:
                  - type: custom:mushroom-update-card
                    entity: update.home_assistant_operating_system_update
                    name: HA Os
                  - type: custom:mushroom-update-card
                    entity: update.home_assistant_core_update
                    name: HA Core
          - type: custom:stack-in-card
            cards:
              - type: horizontal-stack
                cards:
                  - type: custom:mushroom-update-card
                    entity: update.home_assistant_supervisor_update
                    name: HA Supervisor
                  - type: custom:mushroom-update-card
                    entity: update.file_editor_update
                    name: File Editor
          - type: custom:stack-in-card
            cards:
              - type: horizontal-stack
                cards:
                  - type: custom:mushroom-update-card
                    entity: update.adguard_home_update
                    name: Adguard
                  - type: custom:mushroom-update-card
                    entity: update.mosquitto_broker_update
                    name: Mqtt Broker
        style: |
          ha-card {
            margin: 0px 0px 0px !important;
            height: 120px;
          }
      - type: custom:vertical-stack-in-card
        cards:
          - type: custom:decluttering-card
            template: footer_sticky_menu
        style: |
          :host {
            position: sticky !important;
            bottom: 26px;
            margin-bottom: 10px !important;
            animation: 1.2s position ease-in-out;
          }
          @keyframes position {
            0% { bottom: -80px; }
            20% { bottom: -80px; }
            70% { bottom: 26px; }
            90% { bottom: 24px; }
            100% { bottom: 26px; }
          }
          ha-card { 
            background: none;
            border-radius: 26px !important;
            margin-top: 70%;
          }
          :host:before {
            content: '';
            display: block;
            position: absolute;
            bottom: -26px;
            left: -8px;
            padding-right: 16px;
            height: 130px;
            width: 100%; 
            background: linear-gradient(180deg, rgba(45, 56, 76, 0) 0%, rgba(35, 46, 66, 0.85) 50%);
            pointer-events: none;
            animation: 1.2s opacity ease-in-out;
          }
          @keyframes opacity {
            0% { opacity: 0; }
            20% { opacity: 0; }
            100% { opacity: 1; }
          }
          ### The sticky position don't work with Decluttering card
          ### This is why I have to add the CSS here
          ### If you don't use the UI you can use YAML anchors instead
  - theme: noctis
    icon: mdi:auto-fix
    path: auto
    title: Auto
    type: sidebar
    subview: true
    badges: []
    cards:
      - type: custom:decluttering-card
        template: header_temperature_graph
      - type: custom:decluttering-card
        template: header_main
      - type: entities
        entities:
          - type: custom:paper-buttons-row
            buttons:
              - icon: mdi:cog
                layout: icon|name
                name: Automations - Scripts
                style:
                  name:
                    margin-left: 18px
                    font-size: 18px
                    font-weight: 600
            style: |
              div.flex-box {
                margin-top: -6px;
                display: flex;
                justify-content: space-between;
                align-items: center;
              }
          - type: section
            label: Automations
          - type: custom:stack-in-card
            cards:
              - type: custom:layout-card
                layout_type: custom:grid-layout
                layout:
                  grid-template-columns: auto 42px 42px
                  margin: '-4px -4px -8px -4px;'
                cards:
                  - type: custom:mushroom-entity-card
                    entity: automation.home
                    icon: mdi:home
                    icon_color: green
                    fill_container: true
                    card_mod:
                      style: |
                        ha-card {
                          background: none;
                          --ha-card-box-shadow: 0px;
                        }
                  - type: custom:mushroom-template-card
                    entity: automation.home
                    primary: ''
                    secondary: ''
                    icon_color: '{{ ''orange'' if is_state(entity, ''on'') else ''white'' }}'
                    icon: mdi:light-switch
                    hold_action:
                      action: none
                    card_mod:
                      style: |
                        ha-card {
                          align-items: flex-end;
                          background: none;
                          --ha-card-box-shadow: 0px;
                        }
                        mushroom-shape-icon {
                          --shape-color: none !important;
                        }  
                  - type: custom:mushroom-template-card
                    primary: ''
                    secondary: ''
                    icon_color: green
                    icon: mdi:auto-fix
                    tap_action:
                      action: navigate
                      navigation_path: /config/automation/edit/1690024763817
                    card_mod:
                      style: |
                        ha-card {
                          align-items: flex-end;
                          background: none;
                          --ha-card-box-shadow: 0px;
                        }
                        mushroom-shape-icon {
                          --shape-color: none !important;
                        }  
          - type: custom:stack-in-card
            cards:
              - type: custom:layout-card
                layout_type: custom:grid-layout
                layout:
                  grid-template-columns: auto 42px 42px
                  margin: '-4px -4px -8px -4px;'
                cards:
                  - type: custom:mushroom-entity-card
                    entity: automation.leave
                    icon: mdi:home-export-outline
                    fill_container: false
                    card_mod:
                      style: |
                        ha-card {
                          background: none;
                          --ha-card-box-shadow: 0px;
                        }
                  - type: custom:mushroom-template-card
                    entity: automation.leave
                    primary: ''
                    secondary: ''
                    icon_color: '{{ ''orange'' if is_state(entity, ''on'') else ''white'' }}'
                    icon: mdi:light-switch
                    hold_action:
                      action: none
                    card_mod:
                      style: |
                        ha-card {
                          align-items: flex-end;
                          background: none;
                          --ha-card-box-shadow: 0px;
                        }
                        mushroom-shape-icon {
                          --shape-color: none !important;
                        }  
                  - type: custom:mushroom-template-card
                    primary: ''
                    secondary: ''
                    icon_color: green
                    icon: mdi:auto-fix
                    tap_action:
                      action: navigate
                      navigation_path: /config/automation/edit/1690108623379
                    card_mod:
                      style: |
                        ha-card {
                          align-items: flex-end;
                          background: none;
                          --ha-card-box-shadow: 0px;
                        }
                        mushroom-shape-icon {
                          --shape-color: none !important;
                        }  
          - type: custom:stack-in-card
            cards:
              - type: custom:layout-card
                layout_type: custom:grid-layout
                layout:
                  grid-template-columns: auto 42px 42px
                  margin: '-4px -4px -8px -4px;'
                cards:
                  - type: custom:mushroom-entity-card
                    entity: automation.morning
                    icon: mdi:weather-sunny
                    icon_color: yellow
                    fill_container: true
                    card_mod:
                      style: |
                        ha-card {
                          background: none;
                          --ha-card-box-shadow: 0px;
                        }
                  - type: custom:mushroom-template-card
                    entity: automation.morning
                    primary: ''
                    secondary: ''
                    icon_color: '{{ ''orange'' if is_state(entity, ''on'') else ''white'' }}'
                    icon: mdi:light-switch
                    hold_action:
                      action: none
                    card_mod:
                      style: |
                        ha-card {
                          align-items: flex-end;
                          background: none;
                          --ha-card-box-shadow: 0px;
                        }
                        mushroom-shape-icon {
                          --shape-color: none !important;
                        }  
                  - type: custom:mushroom-template-card
                    primary: ''
                    secondary: ''
                    icon_color: green
                    icon: mdi:auto-fix
                    tap_action:
                      action: navigate
                      navigation_path: /config/automation/edit/1690108913531
                    card_mod:
                      style: |
                        ha-card {
                          align-items: flex-end;
                          background: none;
                          --ha-card-box-shadow: 0px;
                        }
                        mushroom-shape-icon {
                          --shape-color: none !important;
                        }  
          - type: custom:stack-in-card
            cards:
              - type: custom:layout-card
                layout_type: custom:grid-layout
                layout:
                  grid-template-columns: auto 42px 42px
                  margin: '-4px -4px -8px -4px;'
                cards:
                  - type: custom:mushroom-entity-card
                    entity: automation.bat_den
                    icon: mdi:globe-light
                    icon_color: orange
                    fill_container: true
                    card_mod:
                      style: |
                        ha-card {
                          background: none;
                          --ha-card-box-shadow: 0px;
                        }
                  - type: custom:mushroom-template-card
                    entity: automation.bat_den
                    primary: ''
                    secondary: ''
                    icon_color: '{{ ''orange'' if is_state(entity, ''on'') else ''white'' }}'
                    icon: mdi:light-switch
                    hold_action:
                      action: none
                    card_mod:
                      style: |
                        ha-card {
                          align-items: flex-end;
                          background: none;
                          --ha-card-box-shadow: 0px;
                        }
                        mushroom-shape-icon {
                          --shape-color: none !important;
                        }  
                  - type: custom:mushroom-template-card
                    primary: ''
                    secondary: ''
                    icon_color: green
                    icon: mdi:auto-fix
                    tap_action:
                      action: navigate
                      navigation_path: /config/automation/edit/1682343608541
                    card_mod:
                      style: |
                        ha-card {
                          align-items: flex-end;
                          background: none;
                          --ha-card-box-shadow: 0px;
                        }
                        mushroom-shape-icon {
                          --shape-color: none !important;
                        }  
          - type: custom:stack-in-card
            cards:
              - type: custom:layout-card
                layout_type: custom:grid-layout
                layout:
                  grid-template-columns: auto 42px 42px
                  margin: '-4px -4px -8px -4px;'
                cards:
                  - type: custom:mushroom-entity-card
                    entity: automation.tat_den
                    icon: mdi:dome-light
                    icon_color: brown
                    fill_container: true
                    card_mod:
                      style: |
                        ha-card {
                          background: none;
                          --ha-card-box-shadow: 0px;
                        }
                  - type: custom:mushroom-template-card
                    entity: automation.tat_den
                    primary: ''
                    secondary: ''
                    icon_color: '{{ ''orange'' if is_state(entity, ''on'') else ''white'' }}'
                    icon: mdi:light-switch
                    hold_action:
                      action: none
                    card_mod:
                      style: |
                        ha-card {
                          align-items: flex-end;
                          background: none;
                          --ha-card-box-shadow: 0px;
                        }
                        mushroom-shape-icon {
                          --shape-color: none !important;
                        }  
                  - type: custom:mushroom-template-card
                    primary: ''
                    secondary: ''
                    icon_color: green
                    icon: mdi:auto-fix
                    tap_action:
                      action: navigate
                      navigation_path: /config/automation/edit/1682343666853
                    card_mod:
                      style: |
                        ha-card {
                          align-items: flex-end;
                          background: none;
                          --ha-card-box-shadow: 0px;
                        }
                        mushroom-shape-icon {
                          --shape-color: none !important;
                        }                  
          - type: section
            label: Scences
          - type: section
            label: Scripts
        style: |
          ha-card {
            margin: 0px 0px 0px !important;
          }
      - type: custom:vertical-stack-in-card
        cards:
          - type: custom:decluttering-card
            template: footer_sticky_menu
        style: |
          :host {
            position: sticky !important;
            bottom: 26px;
            margin-bottom: 10px !important;
            animation: 1.2s position ease-in-out;
          }
          @keyframes position {
            0% { bottom: -80px; }
            20% { bottom: -80px; }
            70% { bottom: 26px; }
            90% { bottom: 24px; }
            100% { bottom: 26px; }
          }
          ha-card { 
            background: none;
            border-radius: 26px !important;
          }
          :host:before {
            content: '';
            display: block;
            position: absolute;
            bottom: -26px;
            left: -8px;
            padding-right: 16px;
            height: 130px;
            width: 100%; 
            background: linear-gradient(180deg, rgba(45, 56, 76, 0) 0%, rgba(35, 46, 66, 0.85) 50%);
            pointer-events: none;
            animation: 1.2s opacity ease-in-out;
          }
          @keyframes opacity {
            0% { opacity: 0; }
            20% { opacity: 0; }
            100% { opacity: 1; }
          }
          ### The sticky position don't work with Decluttering card
          ### This is why I have to add the CSS here
          ### If you don't use the UI you can use YAML anchors instead
  - theme: noctis
    icon: phu:bulb-group-ceiling-round
    path: light
    title: Light
    type: sidebar
    subview: true
    badges: []
    cards:
      - type: custom:decluttering-card
        template: header_temperature_graph
      - type: custom:decluttering-card
        template: header_main
      - type: entities
        entities:
          - type: custom:paper-buttons-row
            buttons:
              - icon: phu:bulb-group-ceiling-round
                layout: icon|name
                name: Light
                style:
                  name:
                    margin-left: 18px
                    font-size: 18px
                    font-weight: 600
              - icon: mdi:close
                layout: icon
                tap_action:
                  action: navigate
                  navigation_path: /
                style:
                  icon:
                    '--mdc-icon-size': 34px
            style: |
              div.flex-box {
                margin-top: -6px;
                display: flex;
                justify-content: space-between;
                align-items: center;
              }
          - type: section
          - type: custom:light-entity-card
            entity: light.yeelink_color4_2917_light
            shorten_cards: false
            consolidate_entities: true
            child_card: true
            hide_header: true
            color_wheel: true
            persist_features: true
            brightness: true
            color_temp: true
            white_value: false
            color_picker: true
            smooth_color_wheel: true
            speed: false
            intensity: false
            force_features: false
            show_slider_percent: false
            full_width_sliders: true
            brightness_icon: weather-sunny
            white_icon: file-word-box
            temperature_icon: thermometer
            speed_icon: speedometer
            intensity_icon: transit-connection-horizontal
            effects_list: false
            style: >
              ha-card {
                margin: 14px 8px !important;
              }

              ha-card > div.light-entity-card__header >
              div.light-entity-card__title {
                font-size: 14px !important;
                font-weight: 500;
              }
          - type: custom:rgb-light-card
            entity: light.yeelink_color4_2917_light
            justify: around
            size: 23
            colors:
              - color_temp: 153
                brightness: 255
                icon_color: white
              - color_temp: 357
                brightness: 255
              - color_temp: 451
                brightness: 255
              - rgb_color:
                  - 255
                  - 170
                  - 0
                brightness: 255
                icon_color: linear-gradient(15deg, rgb(255,170,0), rgb(255,225,165))
              - rgb_color:
                  - 255
                  - 0
                  - 0
                brightness: 255
                icon_color: linear-gradient(15deg, red, rgb(255,100,100))
              - rgb_color:
                  - 244
                  - 38
                  - 255
                brightness: 255
                icon_color: linear-gradient(15deg, rgb(244,38,255), rgb(255,138,255))
              - rgb_color:
                  - 0
                  - 222
                  - 255
                brightness: 255
                icon_color: linear-gradient(15deg, rgb(0,222,255), rgb(140,240,255))
              - rgb_color:
                  - 0
                  - 0
                  - 255
                brightness: 255
                icon_color: linear-gradient(15deg, blue, rgb(100,100,255))
            style: |
              * {
                margin-bottom: 6px !important;
              }
        style: |
          ha-card {
            margin: 0px 0px 0px !important;
          }
  - theme: noctis
    icon: mdi:account
    path: person_1
    title: Person_1
    visible:
      - user: a27a825440914ad7a919367c2e53793e
      - user: aadd52d881af46bca495e279bee867bf
      - user: 10a78a75e63f4c5ba22a81742f59b925
    type: sidebar
    subview: true
    badges: []
    cards:
      - type: custom:decluttering-card
        template: header_temperature_graph
      - type: custom:decluttering-card
        template: header_main
      - type: entities
        entities:
          - type: custom:paper-buttons-row
            buttons:
              - icon: mdi:account
                layout: icon|name
                name: Person
                style:
                  name:
                    margin-left: 18px
                    font-size: 18px
                    font-weight: 600
              - icon: mdi:close
                layout: icon
                tap_action:
                  action: navigate
                  navigation_path: /
                style:
                  icon:
                    '--mdc-icon-size': 34px
            style: |
              div.flex-box {
                margin-top: -6px;
                display: flex;
                justify-content: space-between;
                align-items: center;
              }
          - type: section
          - entity: person.man_1
            type: custom:multiple-entity-row
            name: PM
            state_header: null
          - entity: sensor.iphone_12_mini_battery_level
            type: custom:multiple-entity-row
            name: Battery
            state_header: Battery
          - type: section
          - type: custom:vertical-stack-in-card
            cards:
              - type: history-graph
                entities:
                  - entity: person.man_2
                    name: Present
                hours_to_show: 24
                refresh_interval: 0
                style: |
                  ha-card { 
                    margin: -10px -10px 6px;
                  }
              - type: map
                entities:
                  - entity: person.man_1
                dark_mode: false
                style: |
                  ha-card { 
                    border-radius: 28px !important;
                  }
        style: |
          ha-card {
            margin: 0px 0px 0px !important;
          }
  - theme: noctis
    icon: phu:phoenix-plafond
    path: light_2
    title: Light 2
    visible:
      - user: a27a825440914ad7a919367c2e53793e
      - user: aadd52d881af46bca495e279bee867bf
      - user: 10a78a75e63f4c5ba22a81742f59b925
    type: sidebar
    subview: true
    badges: []
    cards:
      - type: custom:decluttering-card
        template: header_temperature_graph
      - type: custom:decluttering-card
        template: header_main
      - type: entities
        entities:
          - type: custom:paper-buttons-row
            buttons:
              - icon: phu:bulb-group-ceiling-round
                layout: icon|name
                name: Light
                style:
                  name:
                    margin-left: 18px
                    font-size: 18px
                    font-weight: 600
              - icon: mdi:close
                layout: icon
                tap_action:
                  action: navigate
                  navigation_path: /
                style:
                  icon:
                    '--mdc-icon-size': 34px
            style: |
              div.flex-box {
                margin-top: -6px;
                display: flex;
                justify-content: space-between;
                align-items: center;
              }
          - type: section
          - type: custom:light-entity-card
            entity: light.master_light
            shorten_cards: false
            consolidate_entities: true
            child_card: true
            hide_header: true
            color_wheel: true
            persist_features: true
            brightness: true
            color_temp: true
            white_value: false
            color_picker: true
            smooth_color_wheel: true
            speed: false
            intensity: false
            force_features: false
            show_slider_percent: false
            full_width_sliders: true
            brightness_icon: weather-sunny
            white_icon: file-word-box
            temperature_icon: thermometer
            speed_icon: speedometer
            intensity_icon: transit-connection-horizontal
            effects_list: false
            style: >
              ha-card {
                margin: 14px 8px !important;
              }

              ha-card > div.light-entity-card__header >
              div.light-entity-card__title {
                font-size: 14px !important;
                font-weight: 500;
              }
          - type: custom:rgb-light-card
            entity: light.master_light
            size: 23
            colors:
              - color_temp: 153
                brightness: 255
                icon_color: white
              - color_temp: 357
                brightness: 255
              - color_temp: 451
                brightness: 255
              - rgb_color:
                  - 255
                  - 170
                  - 0
                brightness: 255
                icon_color: linear-gradient(15deg, rgb(255,170,0), rgb(255,225,165))
              - rgb_color:
                  - 255
                  - 0
                  - 0
                brightness: 255
                icon_color: linear-gradient(15deg, red, rgb(255,100,100))
              - rgb_color:
                  - 244
                  - 38
                  - 255
                brightness: 255
                icon_color: linear-gradient(15deg, rgb(244,38,255), rgb(255,138,255))
              - rgb_color:
                  - 0
                  - 222
                  - 255
                brightness: 255
                icon_color: linear-gradient(15deg, rgb(0,222,255), rgb(140,240,255))
              - rgb_color:
                  - 0
                  - 0
                  - 255
                brightness: 255
                icon_color: linear-gradient(15deg, blue, rgb(100,100,255))
            style: |
              * {
                margin-bottom: 6px !important;
              }
        style: |
          ha-card {
            margin: 0px 0px 0px !important;
          }
  - theme: noctis
    icon: phu:fan-blade
    path: fan
    title: Fan
    visible:
      - user: a27a825440914ad7a919367c2e53793e
      - user: aadd52d881af46bca495e279bee867bf
      - user: 10a78a75e63f4c5ba22a81742f59b925
    type: sidebar
    subview: true
    badges: []
    cards:
      - type: custom:decluttering-card
        template: header_temperature_graph
      - type: custom:decluttering-card
        template: header_main
      - type: entities
        entities:
          - type: custom:paper-buttons-row
            buttons:
              - icon: mdi:fan
                layout: icon|name
                name: Fan
                style:
                  name:
                    margin-left: 18px
                    font-size: 18px
                    font-weight: 600
              - icon: mdi:close
                layout: icon
                tap_action:
                  action: navigate
                  navigation_path: /
                style:
                  icon:
                    '--mdc-icon-size': 34px
            style: |
              div.flex-box {
                margin-top: -6px;
                display: flex;
                justify-content: space-between;
                align-items: center;
              }
          - type: section
          - type: custom:fan-xiaomi
            entity: fan.dmaker_02_2af0_fan_2
            platform: xiaomi_miio_airpurifier
            force_sleep_mode_support: true
            name: ' '
        style: |
          ha-card {
            margin: 0px 0px 0px !important;
          }
  - theme: noctis
    icon: mdi:air-humidifier
    path: dehumiditya
    title: Demhimidifiera
    visible:
      - user: a27a825440914ad7a919367c2e53793e
      - user: aadd52d881af46bca495e279bee867bf
      - user: 10a78a75e63f4c5ba22a81742f59b925
    type: sidebar
    subview: true
    badges: []
    cards:
      - type: custom:decluttering-card
        template: header_temperature_graph
      - type: custom:decluttering-card
        template: header_main
      - type: entities
        entities:
          - type: custom:paper-buttons-row
            buttons:
              - icon: mdi:air-humidifier
                layout: icon|name
                name: Dehumidifier
                style:
                  name:
                    margin-left: 18px
                    font-size: 18px
                    font-weight: 600
              - icon: mdi:close
                layout: icon
                tap_action:
                  action: navigate
                  navigation_path: /
                style:
                  icon:
                    '--mdc-icon-size': 34px
            style: |
              div.flex-box {
                margin-top: -6px;
                display: flex;
                justify-content: space-between;
                align-items: center;
              }
          - type: section
          - type: custom:stack-in-card
            cards:
              - type: custom:mini-graph-card
                entities:
                  - entity: sensor.dmaker_22l_c3a8_temperature
                    name: Temperature
                    color: '#bb5e00'
                  - entity: sensor.dmaker_22l_c3a8_relative_humidity
                    name: Humidity
                    color: '#2196f3'
                    y_axis: secondary
                    hours_to_show: 24
                    line_width: 3
                    font_size: 50
                    animate: true
                show:
                  name: false
                  icon: false
                  labels: true
                  state: false
                  legend: false
                  fill: fade
              - type: humidifier
                entity: humidifier.dmaker_22l_c3a8_dehumidifier
                name: ' '
        style: |
          ha-card {
            margin: 0px 0px 0px !important;
          }
  - theme: noctis
    icon: mdi:cloud
    path: weather
    title: Weather
    visible:
      - user: a27a825440914ad7a919367c2e53793e
      - user: aadd52d881af46bca495e279bee867bf
      - user: 10a78a75e63f4c5ba22a81742f59b925
    type: sidebar
    subview: true
    badges: []
    cards:
      - type: custom:decluttering-card
        template: header_temperature_graph
      - type: custom:decluttering-card
        template: header_main
      - type: entities
        entities:
          - type: custom:paper-buttons-row
            buttons:
              - icon: mdi:account
                layout: icon|name
                name: Weather
                style:
                  name:
                    margin-left: 18px
                    font-size: 18px
                    font-weight: 600
              - icon: mdi:close
                layout: icon
                tap_action:
                  action: navigate
                  navigation_path: /
                style:
                  icon:
                    '--mdc-icon-size': 34px
            style: |
              div.flex-box {
                margin-top: -6px;
                display: flex;
                justify-content: space-between;
                align-items: center;
              }
          - type: section
          - type: custom:weather-card
            entity: weather.forecast_home
            number_of_forecasts: '5'
        style: |
          ha-card {
            margin: 0px 0px 0px !important;
          }
  - theme: noctis
    icon: phu:apple-tv
    path: tv
    title: tv
    visible:
      - user: a27a825440914ad7a919367c2e53793e
      - user: aadd52d881af46bca495e279bee867bf
      - user: 10a78a75e63f4c5ba22a81742f59b925
    type: sidebar
    subview: true
    badges: []
    cards:
      - type: custom:decluttering-card
        template: header_temperature_graph
      - type: custom:decluttering-card
        template: header_main
      - type: custom:vertical-stack-in-card
        cards:
          - type: entities
            entities:
              - type: custom:paper-buttons-row
                buttons:
                  - icon: mdi:television
                    layout: icon|name
                    name: Apple
                    style:
                      name:
                        margin-left: 18px
                        font-size: 18px
                        font-weight: 600
                  - icon: mdi:close
                    layout: icon
                    tap_action:
                      action: navigate
                      navigation_path: /
                    style:
                      icon:
                        '--mdc-icon-size': 34px
                style: |
                  div.flex-box {
                            margin-top: -6px;
                            display: flex;
                            justify-content: space-between;
                            align-items: center;
                          }
                      - type: section

                        
          - type: picture-elements
            image: local/apple.png
            elements:
              - type: icon
                icon: none
                tap_action:
                  action: navigate
                  navigation_path: home
                style:
                  top: 6%
                  left: 14%
                  transform: translate(-50%, -50%) scale(380%, 380%)
                  opacity: 0
              - type: icon
                icon: none
                tap_action:
                  action: call-service
                  service: script.1689908678448
                style:
                  top: 6%
                  left: 85%
                  transform: translate(-50%, -50%) scale(380%, 380%)
                  opacity: 0
              - type: icon
                icon: none
                tap_action:
                  action: call-service
                  service: remote.send_command
                  service_data:
                    entity_id: remote.tv
                    command: up
                style:
                  top: 23.5%
                  left: 50%
                  transform: translate(-50%, -50%) scale(600%, 800%)
                  opacity: 0
              - type: icon
                icon: none
                tap_action:
                  action: call-service
                  service: remote.send_command
                  service_data:
                    entity_id: remote.tv
                    command: down
                style:
                  top: 62%
                  left: 50%
                  transform: translate(-50%, -50%) scale(600%, 800%)
                  opacity: 0
              - type: icon
                icon: none
                tap_action:
                  action: call-service
                  service: remote.send_command
                  service_data:
                    entity_id: remote.tv
                    command: left
                style:
                  top: 42.6%
                  left: 21%
                  transform: translate(-50%, -50%) scale(600%, 500%)
                  opacity: 0
              - type: icon
                icon: none
                tap_action:
                  action: call-service
                  service: remote.send_command
                  service_data:
                    entity_id: remote.tv
                    command: right
                style:
                  top: 42.6%
                  left: 79%
                  transform: translate(-50%, -50%) scale(600%, 500%)
                  opacity: 0
              - type: icon
                icon: none
                tap_action:
                  action: call-service
                  service: remote.send_command
                  service_data:
                    entity_id: remote.tv
                    command: select
                style:
                  top: 42.4%
                  left: 50%
                  transform: translate(-50%, -50%) scale(400%, 400%)
                  opacity: 0
              - type: icon
                icon: none
                tap_action:
                  action: call-service
                  service: remote.send_command
                  service_data:
                    entity_id: remote.tv
                    command: top_menu
                hold_action:
                  action: call-service
                  service: remote.send_command
                  service_data:
                    entity_id: remote.tv
                    command: home_hold
                style:
                  top: 80%
                  left: 15.5%
                  transform: translate(-50%, -50%) scale(380%, 380%)
                  opacity: 0
              - type: icon
                icon: none
                tap_action:
                  action: call-service
                  service: media_player.media_play_pause
                  service_data:
                    entity_id: media_player.tv
                style:
                  top: 91%
                  left: 15.5%
                  transform: translate(-50%, -50%) scale(380%, 380%)
                  opacity: 0
              - type: icon
                icon: none
                tap_action:
                  action: call-service
                  service: remote.send_command
                  service_data:
                    entity_id: remote.tv
                    command: menu
                style:
                  top: 85.5%
                  left: 50%
                  transform: translate(-50%, -50%) scale(750%, 750%)
                  opacity: 0
              - type: icon
                icon: none
                tap_action:
                  action: call-service
                  service: remote.send_command
                  service_data:
                    entity_id: remote.tv
                    command: volume_up
                style:
                  top: 80%
                  left: 84.5%
                  transform: translate(-50%, -50%) scale(380%, 380%)
                  opacity: 0
              - type: icon
                icon: none
                tap_action:
                  action: call-service
                  service: remote.send_command
                  service_data:
                    entity_id: remote.tv
                  command: volume_down
                style:
                  top: 91%
                  left: 84.5%
                  transform: translate(-50%, -50%) scale(380%, 380%)
                  opacity: 0
            card_mod:
              style: |
                .type-picture-elements {
                min-height: 100px; 
                max-height: 640px;
                min-width: 100px;
                max-width: 400px; 
                }
  - theme: noctis
    icon: phu:roborock
    path: roborock_temp
    title: roborock
    visible:
      - user: a27a825440914ad7a919367c2e53793e
      - user: aadd52d881af46bca495e279bee867bf
      - user: 10a78a75e63f4c5ba22a81742f59b925
    type: sidebar
    subview: true
    badges: []
    cards:
      - type: custom:decluttering-card
        template: header_temperature_graph
      - type: custom:decluttering-card
        template: header_main
      - type: entities
        entities:
          - type: custom:paper-buttons-row
            buttons:
              - icon: phu:roborock
                layout: icon|name
                name: Roborock
                style:
                  name:
                    margin-left: 18px
                    font-size: 18px
                    font-weight: 600
              - icon: mdi:close
                layout: icon
                tap_action:
                  action: navigate
                  navigation_path: /
                style:
                  icon:
                    '--mdc-icon-size': 34px
            style: |
              div.flex-box {
                margin-top: -6px;
                display: flex;
                justify-content: space-between;
                align-items: center;
              }
          - type: section
          - type: custom:xiaomi-vacuum-card
            entity: vacuum.roborock_s5_420f_robot_cleaner_2
            image: /local/roborock.jpg
            name: false
            vendor: xiaomi
            state:
              status:
                key: state
            attributes:
              main_brush: null
              side_brush: null
              sensor: false
              filter: false
              clean_area:
                key: clean_area
                label: 'Cleaned area: '
                unit: ' m2'
              clean_time:
                key: clean_time
                label: 'Time clean: '
                unit: ' min'
            buttons: true
        style: |
          ha-card {
            margin: 0px 0px 0px !important;
          }
  - theme: noctis
    icon: phu:air-conditioner
    path: ac
    title: Climate
    type: sidebar
    subview: true
    badges: []
    cards:
      - type: custom:decluttering-card
        template: header_temperature_graph
      - type: custom:decluttering-card
        template: header_main
      - type: entities
        entities:
          - type: custom:paper-buttons-row
            buttons:
              - icon: phu:air-conditioner
                layout: icon|name
                name: Climate
                style:
                  name:
                    margin-left: 18px
                    font-size: 18px
                    font-weight: 600
              - icon: mdi:close
                layout: icon
                tap_action:
                  action: navigate
                  navigation_path: /
                style:
                  icon:
                    '--mdc-icon-size': 34px
            style: |
              div.flex-box {
                margin-top: -6px;
                display: flex;
                justify-content: space-between;
                align-items: center;
              }
          - type: section
          - type: custom:stack-in-card
            cards:
              - type: custom:mini-graph-card
                entities:
                  - entity: sensor.aqara_temperature_and_humidity_sensor_temperature
                    name: Temperature
                    color: '#bb5e00'
                  - entity: sensor.aqara_temperature_and_humidity_sensor_humidity
                    name: Humidity
                    color: '#00bb33'
                    y_axis: secondary
                    hours_to_show: 24
                    line_width: 3
                    font_size: 50
                    animate: true
                show:
                  name: false
                  icon: false
                  labels: true
                  state: false
                  legend: false
                  fill: fade
              - type: thermostat
                entity: climate.pm
                name: ' '
        style: |
          ha-card {
            margin: 0px 0px 0px !important;
          }
  - title: Master
    path: master
    icon: mdi:bed-double
    theme: noctis
    type: sidebar
    subview: true
    badges: []
    cards:
      - type: custom:decluttering-card
        template: header_temperature_graph
      - type: custom:decluttering-card
        template: header_main
      - type: custom:vertical-stack-in-card
        cards:
          - type: entities
            entities:
              - type: custom:paper-buttons-row
                buttons:
                  - icon: mdi:bed-double-outline
                    layout: icon|name
                    name: Master
                    style:
                      name:
                        margin-left: 18px
                        font-size: 18px
                        font-weight: 600
                  - icon: mdi:close
                    layout: icon
                    tap_action:
                      action: navigate
                      navigation_path: /dashboard-a/room
                    style:
                      icon:
                        '--mdc-icon-size': 40px
                style: |
                  div.flex-box {
                  margin-top: -6px;
                  display: flex;
                  justify-content: space-between;
                  align-items: center;
                  }
              - type: section
              - type: custom:vertical-stack-in-card
                cards:
                  - type: custom:swipe-card
                    cards:
                      - type: horizontal-stack
                        cards:
                          - type: custom:mushroom-template-card
                            layout: vertical
                            entity: >-
                              sensor.aqara_temperature_and_humidity_sensor_temperature
                            icon: mdi:thermometer
                            primary: '{{ states(entity) | round(1) }}°C'
                            icon_color: orange
                          - type: custom:mushroom-template-card
                            layout: vertical
                            entity: >-
                              sensor.aqara_temperature_and_humidity_sensor_humidity
                            icon: mdi:water-percent
                            primary: '{{ states(entity) | round(1)  }}%'
                            icon_color: blue
                          - type: custom:mushroom-template-card
                            layout: vertical
                            entity: binary_sensor.aqara_motion_sensor_p1_occupancy
                            primary: |
                              {% if is_state(entity, 'off') %}
                              No Motion
                              {% else %}
                              Motion
                              {% endif %}
                            icon_color: |
                              {% if is_state(entity, 'off') %}

                              {% else %}
                              orange
                              {% endif %}
                            icon: |
                              {% if is_state(entity, 'off') %}
                              mdi:run
                              {% else %}
                              mdi:run-fast
                              {% endif %}
                            secondary: ''
                            card_mod:
                              style: >
                                mushroom-shape-icon {

                                {{ 'animation: blink 1.3s ease-in-out infinite;'
                                if
                                is_state('binary_sensor.aqara_motion_sensor_p1_occupancy',
                                'on') }}

                                }

                                @keyframes blink {

                                50% {opacity: 0.1;}

                                }
                      - type: horizontal-stack
                        cards:
                          - type: custom:mushroom-template-card
                            layout: vertical
                            entity: binary_sensor.aqara_door_and_window_sensor_door
                            primary: |
                              {% if is_state(entity, 'off') %}
                              Close
                              {% else %}
                              Open
                              {% endif %}
                            icon_color: |
                              {% if is_state(entity, 'off') %}
                              white
                              {% else %}
                              green
                              {% endif %}
                            icon: |
                              {% if is_state(entity, 'off') %}
                              mdi:door
                              {% else %}
                              mdi:door-closed
                              {% endif %}
                            secondary: ''
                            card_mod: null
                            style: >
                              mushroom-shape-icon { {{ 'animation: blink 1.3s
                              ease-in-out infinite;' if
                              is_state('binary_sensor.aqara_door_and_window_sensor_door',
                              'on') }} } @keyframes blink { 50% {opacity: 0.1;}
                              }
              - type: custom:mushroom-media-player-card
                entity: media_player.tv
                icon: phu:apple-tv
                volume_controls: []
                media_controls:
                  - previous
                  - play_pause_stop
                  - next
                layout: horizontal
                use_media_info: true
                show_volume_level: true
              - type: custom:mushroom-climate-card
                entity: climate.pm
                name: Sensibo Air
                icon: phu:air-conditioner
                layout: horizontal
                hvac_modes:
                  - cool
                  - 'off'
                show_temperature_control: true
                icon_type: icon
              - type: custom:mushroom-cover-card
                entity: cover.aqara_curtain_controller
                icon: phu:vert-blind-open
                show_tilt_position_control: false
                show_buttons_control: true
                name: Curtain
                layout: horizontal
                fill_container: true
              - type: custom:mushroom-vacuum-card
                entity: vacuum.roborock_s5_420f_robot_cleaner_2
                icon: phu:roborock
                icon_animation: true
                layout: horizontal
                commands:
                  - start_pause
                  - stop
                  - return_home
                name: Sen
              - type: custom:mushroom-light-card
                entity: light.master_light
                name: Master
                icon: mdi:light-recessed
                layout: horizontal
                show_brightness_control: true
        style: |
          ha-card {
            margin: 0px 0px 0px !important;
          }
  - title: living
    path: living
    icon: mdi:desk
    type: sidebar
    subview: true
    theme: noctis
    badges: []
    cards:
      - type: custom:decluttering-card
        template: header_temperature_graph
      - type: custom:decluttering-card
        template: header_main
      - type: custom:vertical-stack-in-card
        cards:
          - type: entities
            entities:
              - type: custom:paper-buttons-row
                buttons:
                  - icon: mdi:sofa-single
                    layout: icon|name
                    name: Other
                    style:
                      name:
                        margin-left: 18px
                        font-size: 18px
                        font-weight: 600
                  - icon: mdi:close
                    layout: icon
                    tap_action:
                      action: navigate
                      navigation_path: /dashboard-a/room
                    style:
                      icon:
                        '--mdc-icon-size': 40px
                style: |
                  div.flex-box {
                  margin-top: -6px;
                  display: flex;
                  justify-content: space-between;
                  align-items: center;
                  }
              - type: section
              - type: custom:stack-in-card
                cards:
                  - type: custom:layout-card
                    layout_type: custom:grid-layout
                    layout:
                      grid-template-columns: auto 42px 42px
                      margin: '-4px -4px -8px -2px;'
                    cards:
                      - type: custom:mushroom-media-player-card
                        entity: media_player.hanh_lang
                        use_media_info: false
                        name: Lobby
                        icon: phu:home-mini
                        show_volume_level: false
                        volume_controls:
                          - volume_set
                        layout: horizontal
                        fill_container: true
                        card_mod:
                          style: |
                            ha-card {
                              background: none;
                              --ha-card-box-shadow: 0px;
                            }
                      - type: custom:mushroom-template-card
                        primary: ''
                        secondary: ''
                        icon: mdi:spotify
                        icon_color: disabled
                        tap_action:
                          action: call-service
                          service: spotcast.start
                          data:
                            force_playback: false
                            random_song: false
                            shuffle: false
                            offset: 0
                            ignore_fully_played: false
                            entity_id: media_player.hanh_lang
                        card_mod:
                          style: |
                            ha-card {
                              align-items: flex-end;             
                              background: none;
                              --ha-card-box-shadow: 0px;
                            }
                            mushroom-shape-icon {
                              --shape-color: none !important;
                            } 
                      - type: custom:mushroom-template-card
                        entity: input_boolean.media_dropdown
                        primary: ''
                        secondary: ''
                        icon: >-
                          {{ 'mdi:chevron-up' if is_state(entity, 'playing')
                          else 'mdi:chevron-down' }}
                        icon_color: disabled
                        hold_action:
                          action: none
                        card_mod:
                          style: |
                            ha-card {
                              align-items: flex-end;
                              background: none;
                              --ha-card-box-shadow: 0px;
                            }
                            mushroom-shape-icon {
                              --shape-color: none !important;
                            }    
                  - type: conditional
                    conditions:
                      - entity: input_boolean.media_dropdown
                        state: 'on'
                    card:
                      type: custom:stack-in-card
                      cards:
                        - type: custom:mushroom-media-player-card
                          entity: media_player.hanh_lang
                          icon: mdi:music
                          use_media_info: true
                          use_media_artwork: false
                          how_volume_level: false
                          show_icon: false
                          media_controls:
                            - play_pause_stop
                            - previous
                            - next
                          fill_container: true
                          card_mod:
                            style:
                              mushroom-state-info$: |
                                .primary {
                                  white-space: normal !important;
                                  font-size: 18px !important;
                                  font-weight: 100;
                                }
                                .secondary {
                                  white-space: normal !important;
                                  font-size: 15px !important;
                                  font-weight: 200;
                                }
                              .: |
                                mushroom-shape-icon {
                                  display: none ;
                                  icon-color: white;
                                }
                                ha-card {
                                  --ha-card-border-width: 0;
                                  --control-icon-size: 32px;
                                  background-color: transparent;
                                  margin-top: 10px;
                                  box-shadow: none;
                                }
                                ha-card:before {
                                  transform: translate3d(0,0,0);
                                  -webkit-transform: translate3d(0,0,0);
                                  content: "";
                                  {% if not is_state(config.entity, 'off') %}
                                    background: url( '{{ state_attr(config.entity, "entity_picture") }}') center no-repeat;
                                  {% else %}
                                    background: url('/local/Spotify_c.jpg') center no-repeat;
                                  {% endif %}
                                  background-size: contain;
                                  width: 20rem;
                                  align-self: center;
                                  margin: 40px 40px 28px;
                                  filter: drop-shadow(4px 4px 6px rgba(0, 0, 0, 0));
                                  border-radius: 30px;
                                  {% set media_type = state_attr(config.entity, 'media_content_type') %}
                                  {% if media_type == 'music' %}
                                    aspect-ratio: 16 / 9;
                                  {% elif media_type == 'movie' %}
                                    aspect-ratio: 2 / 3;
                                  {% else %}
                                    aspect-ratio: 1 / 1;
                                  {% endif %}
                                }
                                mushroom-state-item {
                                  text-align: center;
                                  margin-left: -10px;
                                }
                                .actions {
                                   --rgb-primary-text-color: var(--album-art-color);
                                   --primary-text-color: rgb(var(--album-art-color));
                                   {% if state_attr(config.entity, 'media_duration') %}
                                   margin-top: 55px;
                                   {% else %}
                                   margin-top: 20px;
                                   {% endif %}
                                   margin-left: -20px; 
                                   width: 80%;
                                   align-self: center;
                                }
                        - type: custom:mini-media-player
                          entity: media_player.hanh_lang
                          hide:
                            icon: true
                            name: true
                            runtime: true
                            source: true
                            power: true
                            state_label: true
                            volume: true
                            info: true
                            progress: false
                            controls: true
                          more_info: false
                          toggle_power: false
                          group: true
                          card_mod:
                            style:
                              mmp-progress$: |
                                paper-progress {
                                  {% if is_state(config.entity, 'playing') %}
                                  --paper-progress-container-color: amber ;
                                  {% endif %}
                                }
                              .: |
                                ha-card {
                                  width: 60%;
                                  align-self: center;
                                  margin: -98px auto auto;
                                  --mmp-progress-height: 12px !important;
                                  height: var(--mmp-progress-height);
                                  --mmp-accent-color: white;
                                  --mmp-border-radius: 12px !important;
                                  --ha-card-border-width: 0;
                                  background-color: transparent !important;
                                }
              - type: custom:mushroom-fan-card
                entity: fan.server_fan
                name: Server
                icon: phu:pedastal-fan
                layout: horizontal
                show_percentage_control: true
                show_oscillate_control: true
        style: |
          ha-card {
            margin: 0px 0px 0px !important;
          }
  - title: Mom
    path: mom
    icon: mdi:bed-queen
    theme: noctis
    type: sidebar
    subview: true
    badges: []
    cards:
      - type: custom:decluttering-card
        template: header_temperature_graph
      - type: custom:decluttering-card
        template: header_main
      - type: entities
        entities:
          - type: custom:paper-buttons-row
            buttons:
              - icon: mdi:bed-king
                layout: icon|name
                name: Mom
                style:
                  name:
                    margin-left: 18px
                    font-size: 18px
                    font-weight: 600
              - icon: mdi:close
                layout: icon
                tap_action:
                  action: navigate
                  navigation_path: /dashboard-a/room
                style:
                  icon:
                    '--mdc-icon-size': 34px
            style: |
              div.flex-box {
              margin-top: -6px;
              display: flex;
              justify-content: space-between;
              align-items: center;
              }
          - type: section
          - type: custom:vertical-stack-in-card
            cards:
              - type: horizontal-stack
                cards:
                  - type: custom:mushroom-template-card
                    layout: vertical
                    entity: sensor.cleargrass_dk1_ae69_temperature_humidity_sensor
                    icon: mdi:thermometer
                    primary: '{{ states(entity) | capitalize }}°C'
                    icon_color: orange
                  - type: custom:mushroom-template-card
                    layout: vertical
                    entity: sensor.cleargrass_dk1_ae69_relative_humidity
                    icon: mdi:water-percent
                    primary: '{{ states(entity) | capitalize }}%'
                    icon_color: blue
                  - type: custom:mushroom-template-card
                    layout: vertical
                    entity: sensor.illumination_04cf8ca07ea5
                    icon: mdi:white-balance-sunny
                    primary: '{{ states(entity) | capitalize }}lux'
                    icon_color: white
          - type: custom:mushroom-climate-card
            entity: climate.ac
            name: AC
            icon: phu:air-conditioner
            layout: horizontal
            hvac_modes:
              - cool
              - dry
            show_temperature_control: true
            icon_type: icon
          - type: custom:mushroom-light-card
            entity: light.gateway_light_04cf8ca07ea5
            name: Hub
            icon: mdi:light-recessed
            layout: horizontal
            use_light_color: true
            show_brightness_control: true
            show_color_control: true
  - title: ac2
    path: ac2
    icon: mdi:bed-single
    theme: noctis
    type: sidebar
    subview: true
    badges: []
    cards:
      - type: custom:decluttering-card
        template: header_temperature_graph
      - type: custom:decluttering-card
        template: header_main
      - type: entities
        entities:
          - type: custom:paper-buttons-row
            buttons:
              - icon: mdi:bed-king
                layout: icon|name
                name: AC2
                style:
                  name:
                    margin-left: 18px
                    font-size: 18px
                    font-weight: 600
              - icon: mdi:close
                layout: icon
                tap_action:
                  action: navigate
                  navigation_path: /dashboard-a/room
                style:
                  icon:
                    '--mdc-icon-size': 34px
            style: |
              div.flex-box {
              margin-top: -6px;
              display: flex;
              justify-content: space-between;
              align-items: center;
              }
          - type: section
          - type: custom:vertical-stack-in-card
            cards:
              - type: horizontal-stack
                cards:
                  - type: custom:mushroom-template-card
                    layout: vertical
                    entity: sensor.cleargrass_dk1_b2a6_temperature_humidity_sensor
                    icon: mdi:thermometer
                    primary: '{{ states(entity) | capitalize }}°C'
                    icon_color: orange
                  - type: custom:mushroom-template-card
                    layout: vertical
                    entity: sensor.cleargrass_dk1_b2a6_relative_humidity
                    icon: mdi:water-percent
                    primary: '{{ states(entity) | capitalize }}%'
                    icon_color: blue
                  - type: custom:mushroom-template-card
                    layout: vertical
                    entity: sensor.illumination_04cf8c9151ce
                    icon: mdi:white-balance-sunny
                    primary: '{{ states(entity) | capitalize }}lux'
                    icon_color: white
          - type: custom:mushroom-light-card
            entity: light.gateway_light_04cf8c9151ce
            name: Hub
            icon: mdi:light-recessed
            layout: horizontal
            use_light_color: true
            show_brightness_control: true
            show_color_control: true
          - type: custom:mushroom-media-player-card
            entity: media_player.md_homepod
            icon: phu:apple-tv
            volume_controls: []
            media_controls:
              - previous
              - play_pause_stop
              - next
            layout: horizontal
            use_media_info: true
            show_volume_level: true
  - theme: noctis
    icon: phu:roborock
    path: roborock
    title: Roborock
    visible:
      - user: a27a825440914ad7a919367c2e53793e
      - user: aadd52d881af46bca495e279bee867bf
      - user: 10a78a75e63f4c5ba22a81742f59b925
    type: sidebar
    subview: true
    badges: []
    cards:
      - type: custom:decluttering-card
        template: header_temperature_graph
      - type: custom:decluttering-card
        template: header_main
      - type: entities
        entities:
          - type: custom:paper-buttons-row
            buttons:
              - icon: phu:roborock
                layout: icon|name
                name: Roborock
                style:
                  name:
                    margin-left: 18px
                    font-size: 18px
                    font-weight: 600
              - icon: mdi:close
                layout: icon
                tap_action:
                  action: navigate
                  navigation_path: /
                style:
                  icon:
                    '--mdc-icon-size': 34px
            style: |
              div.flex-box {
                margin-top: -6px;
                display: flex;
                justify-content: space-between;
                align-items: center;
              }
          - type: custom:xiaomi-vacuum-map-card
            language: en
            preset_name: Roborock S5
            entity: vacuum.robot_don_nha
            map_source:
              camera: camera.my_vacuum_camera
            map_locked: false
            calibration_source:
              camera: true
            vacuum_platform: default
            style: >
              ha-card > div.controls-wrapper > div.vacuum-controls > div {

                color: var(--map-card-internal-primary-color);

              }

              ha-card > div.controls-wrapper > div.map-controls-wrapper > div >
              div {
                background: white;
              }
            card_mod:
              style: |
                ha-card {
                  background-color: var(--primary-background-color);
                  background: rgb(0, 49, 83, 0.4);
                  box-shadow: none;
                  }
        style: |
          ha-card {
            margin: 0px 0px 0px !important;
          }
      - type: custom:vertical-stack-in-card
        cards:
          - type: custom:decluttering-card
            template: footer_sticky_menu
        style: |
          :host {
            position: sticky !important;
            bottom: 26px;
            margin-bottom: 10px !important;
            animation: 1.2s position ease-in-out;
          }
          @keyframes position {
            0% { bottom: -80px; }
            20% { bottom: -80px; }
            70% { bottom: 26px; }
            90% { bottom: 24px; }
            100% { bottom: 26px; }
          }
          ha-card { 
            background: none;
            border-radius: 26px !important;
            margin-top: 70%;
          }
          :host:before {
            content: '';
            display: block;
            position: absolute;
            bottom: -26px;
            left: -8px;
            padding-right: 16px;
            height: 130px;
            width: 100%; 
            background: linear-gradient(180deg, rgba(45, 56, 76, 0) 0%, rgba(35, 46, 66, 0.85) 50%);
            pointer-events: none;
            animation: 1.2s opacity ease-in-out;
          }
          @keyframes opacity {
            0% { opacity: 0; }
            20% { opacity: 0; }
            100% { opacity: 1; }
          }
