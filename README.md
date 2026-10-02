# YUDX Tool Changer

An open-source automatic tool-changing system for 3D printers that enables **1-second tool changes with almost zero waste**.

## ✨ Key Features

- **~1 second tool change** — near-instant switching between tool heads, and dramatically reducing purge/waste material compared to traditional multi-material setups.
- **Preheatable tool heads** — each tool head has its own independent heater and thermistor, so it can be pre-heated in advance while parked.
- **Simple, reliable mechanism** — the tool-docking mechanism is based on a one-way bearing design, which keeps the system mechanically simple while remaining stable and reliable.

## 🖨️ Compatibility

Currently designed as a conversion for:
- **Voron 2.4**
- **Voron Trident**

Support for additional printer platforms may be added in the future.

## 👤 Author

It's just me — **dumpling**. No company, no team, just one person working on this.

I'm a student in Shanghai, just starting my third year of university, and I've loved DIY 3D printers since middle school, designing and building my own machines for years.

Since this is a one-person project that I work on in my spare time, updates may come slowly — thanks for your patience and interest!

## 🧵 Firmware & G-code

The G-code provided in this repository is generated for **RepRapFirmware (RRF)**.

If you are running **Klipper**, you will need to write your own tool-change macros/G-code for now. If you do write a working Klipper configuration, contributions are very welcome — please consider opening a pull request so other Klipper users can benefit.

## 💬 Community

Join the Discord: [https://discord.gg/huHw8kzNXp](https://discord.gg/huHw8kzNXp)

## 🔧 Installation

See [Assembly Instructions](<./YUDX Assembly Instruction.pdf>) for the full build and installation guide.

## 📦 Bill of Materials

See [BOM.xlsx](<./"YUDX"BOM.xlsx>) for the complete parts list.

## 📄 License

See the [LICENSE](./LICENSE) file for details.

## 📖 Project Background

I've been developing an automatic tool-changing 3D printer since 2024. Initially, I experimented with a design similar to the Prusa XL's tool-changing approach.

Everything changed in April 2025, when I saw the reveal video for Bondtech's INDX system. I was blown away, and it pushed me to take the design in a new direction. Many thanks to Bondtech for the inspiration.

I explored a number of different mechanical designs. Then, one night in May 2025, the idea for the current one-way bearing mechanism came to me in a dream — a solution that turned out to be simple, stable, and reliable. Since building and machining the first prototype, the design has gone through continuous refinement, and reliability is now solid.

## 🙏 Acknowledgments

- **Bondtech** — for the INDX system, which was a huge inspiration for the tool-changing approach used in this project.
- **whyme12** — for helping add screws and nuts, and for completing the BOM spreadsheet.
- **Voron Design** — the open-source Voron project that this system is built on top of.
- **RepRapFirmware (RRF)** — the open-source firmware this project's G-code is developed for.

## 🤝 Contributing

Contributions are welcome, especially:
- Klipper-compatible tool-change G-code / macros
- Bug reports and reliability improvements
- Documentation improvements

Feel free to open an issue or pull request.
