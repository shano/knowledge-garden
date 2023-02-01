My primary keyboard is a [Corne Keyboard](https://github.com/foostan/crkbd) with [Helidox Key Caps](https://mechboards.co.uk/products/helidox-corne-kit).

- The [miryoku layout](https://github.com/manna-harbour/miryoku) looks like something worth trying. I'm finding the default layout really problematic.
-
- Short Guide to writing your own layout
	- Copy some existing layout and make your tweak to the keymap.c file
	- Test by compiling: `qmk compile -kb crkbd -km md`
	- Flash to your keyboard: `qmk flash -kb crkbd -km md`
	- If you're happy convert it to json: `qmk c2json -kb crkbd -km md keyboards/crkbd/keymaps/md/keymap.c >> md.json`
	- Upload to https://config.qmk.fm/ and print.