# Set Default Layer Behavior

This is behavior allow to set default layer to an alternaive in higher layer index for ZMK with at least Zephyr 3.5.

## What it does

This behavior change the value of `_zmk_keymap_layer_default` in keymap.c to break layer loop in given layer index. The value shoule be a layer index where >0 and less than the first functional layer.

## Installation

Include this project on your ZMK's west manifest in `config/west.yml`:

```yaml
manifest:
  remotes:
    - name: zmkfirmware
      url-base: https://github.com/zmkfirmware
    - name: badjeff
      url-base: https://github.com/badjeff
  projects:
    - name: zmk
      remote: zmkfirmware
      revision: main
      import: app/west.yml
    # START #####
    - name: zmk-behavior-set-default-layer
      remote: badjeff
      revision: main
    # END #######
  self:
    path: config
```

Now, update your `shield.keymap` adding the behaviors.

```keymap
/ {
        behaviors {
                sdl: set_def_layer {
                        compatible = "zmk,behavior-set-default-layer";
                        #binding-cells = <1>;
                };
        };

        keymap {
                compatible = "zmk,keymap";
                mac_layer {
                        // index 0 - default!
                        bindings = <
                              &sdl 1 // set default to win_layer
                              &SUPER &kp C &kp V
                        >;
                };
                win_layer {
                        // index 1
                        bindings = <
                              &sdl 0 // set default to win_layer
                              &LCTRL &kp C &kp V
                        >;
                };
                num_layer {
                        // index 3
                        bindings = <
                                .... .... .... ....
                        >;
                }
       };

};
```
