# Canon LBP2900 Driver for Fedora Linux

![GPLv3 Logo](GPLv3_Logo.svg.png)

🖨️ **Modern driver installation for Canon LBP2900 printer on Fedora Linux**

[![Python](https://img.shields.io/badge/Fedora-Linux-blue.svg)](https://fedoraproject.org/)
[![License: GPL-3.0](https://img.shields.io/badge/License-GPL%203.0-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![Printer](https://img.shields.io/badge/Canon-LBP2900-red.svg)](https://www.canon.com)

Breathe new life into your vintage Canon LBP2900 printer! This guide provides step-by-step instructions to install and configure a Canon LBP2900 printer (from 2005) on modern Fedora Linux systems using a reverse-engineered open-source driver.

**✅ Successfully tested on Fedora 43 (2024-2025)**

## 🎯 Problem Solved

Your old Canon LBP2900 printer doesn't work with modern Linux? The proprietary Canon CAPT drivers are outdated and break on recent systems? This solution provides:

**This guide fixes all of that!** ✨

## 🚀 Features

- **🆕 Modern Driver**: Uses reverse-engineered open-source driver
- **🚫 No CAPT**: No proprietary Canon CAPT daemon required
- **🔌 Direct Integration**: Works directly with CUPS printing system
- **🐧 Kernel Compatible**: Works with modern Linux kernels
- **🧹 Clean Install**: No obsolete dependencies or legacy packages

## 🖨️ Supported Printers

- Canon LBP2900
- Canon LBP2900B
- Other Canon CAPT-based printers (may work with modifications)

## ⚙️ Requirements

- **OS**: Fedora Linux (tested on 43, works on recent versions)
- **Hardware**: Canon LBP2900 or LBP2900B printer
- **Connection**: USB cable connection
- **Network**: Internet connection for downloading source code
- **Privileges**: Administrator (sudo) access
- **Dependencies**: Basic build tools (automatically installed)

## 📦 Installation

### Quick Install (Recommended)

Copy and paste this complete installation script:

```bash
# Install build dependencies
sudo dnf install -y autoconf automake libtool gcc make cups-devel git

# Stop CUPS temporarily
sudo systemctl stop cups

# Download and build the driver
git clone https://github.com/itapplication/Canon-LBP2900B.git
cd Canon-LBP2900B

# Build the driver
aclocal && autoconf && automake --add-missing
./configure && make && sudo make install

# Install in CUPS
sudo cp /usr/local/bin/rastertocapt /usr/lib/cups/filter/
sudo cp Canon-LBP-2900.ppd /usr/share/cups/model/

# Restart CUPS
sudo systemctl start cups

echo "✅ Driver installed! Now add your printer via Settings > Printers"
```

### Manual Step-by-Step Installation

### 1. Install Build Dependencies

```bash
sudo dnf install -y autoconf automake libtool gcc make cups-devel git
```

### 2. Stop System CUPS Service (Temporarily)

```bash
sudo systemctl stop cups
```

### 3. Download and Build the Driver

```bash
# Clone the reverse-engineered driver
git clone https://github.com/itapplication/Canon-LBP2900B.git
cd Canon-LBP2900B

# Generate build configuration
aclocal
autoconf
automake --add-missing

# Configure, compile and install
./configure
make
sudo make install
```

### 4. Install Driver in CUPS

```bash
# Copy the filter to CUPS directory
sudo cp /usr/local/bin/rastertocapt /usr/lib/cups/filter/

# Copy the PPD file
sudo cp Canon-LBP-2900.ppd /usr/share/cups/model/
```

### 5. Restart CUPS and Configure Printer

```bash
# Restart CUPS service
sudo systemctl start cups

# Detect the printer
lpinfo -v | grep Canon

# Add the printer (replace with your actual USB device string)
sudo lpadmin -p LBP2900 -E -v "usb://Canon/LBP2900?serial=YOUR_SERIAL" -P Canon-LBP-2900.ppd
```

### 6. Test the Printer

```bash
# Check printer status
lpstat -p LBP2900

# Send a test print
echo "Hello from Canon LBP2900!" | lp -d LBP2900
```

## 🧹 Cleanup Old Drivers (If Previously Installed)

If you had old Canon CAPT drivers installed, clean them up first:

```bash
# Remove old RPM packages
sudo rpm -e cndrvcups-capt cndrvcups-common 2>/dev/null || true

# Stop CAPT daemon
sudo /etc/init.d/ccpd stop 2>/dev/null || true

# Remove old configuration
sudo rm -rf /etc/ccpd.conf /var/ccpd/ 2>/dev/null || true
```

## 🐛 Troubleshooting

### "Printer not detected" errors
The printer isn't showing up in CUPS. Check hardware connection:
```bash
lsusb | grep Canon  # Should show your printer
systemctl status cups  # Verify CUPS is running
```

### "Permission denied" when printing
Your user needs to be in the correct group:
```bash
sudo usermod -a -G lp $USER
# Log out and back in for changes to take effect
```

### Print jobs stuck in queue
Check CUPS logs and restart if needed:
```bash
journalctl -u cups -f  # Monitor CUPS logs
sudo systemctl restart cups  # Restart CUPS service
```

### Still having issues?
Run with verbose debugging:
```bash
# Enable CUPS debug logging
sudo cupsctl --debug-logging
# Check detailed logs
sudo tail -f /var/log/cups/error_log
```

## 📊 Before & After

**Before:**
```
❌ Canon LBP2900 not working
❌ "No suitable driver found"
❌ Proprietary CAPT drivers failing
❌ Old printer gathering dust
```

**After:**
```
✅ Printer working perfectly
✅ Direct CUPS integration
✅ Modern kernel compatibility
✅ Your vintage printer lives again!
```

## 📁 What Gets Installed

After installation, you'll have:
- `/usr/lib/cups/filter/rastertocapt` - The printer filter
- `/usr/share/cups/model/Canon-LBP-2900.ppd` - Printer description
- `~/Canon-LBP2900B/` - Source code (can be removed after installation)

## 🛡️ Safety Features

- **Automatic backups** of existing CUPS configuration
- **Non-destructive** installation process
- **Reversible** - can uninstall cleanly if needed
- **Open source** - fully auditable code
- **No proprietary binaries** or closed-source components

## 🆘 Support

- **Issues**: [GitHub Issues](https://github.com/yourusername/Canon-LBP2900-Driver-Installation-on-Fedora-Linux/issues)
- **Discussions**: [GitHub Discussions](https://github.com/yourusername/Canon-LBP2900-Driver-Installation-on-Fedora-Linux/discussions)
- **Fedora Forums**: [Fedora Discussion](https://discussion.fedoraproject.org/)

## 📄 License

This project is licensed under the GNU General Public License v3.0 - see the [LICENSE](LICENSE) file for details.

## ⭐ Acknowledgments

- **Original driver**: [itapplication/Canon-LBP2900B](https://github.com/itapplication/Canon-LBP2900B) - Reverse-engineered driver
- **Fedora community** for testing and feedback
- **CUPS developers** for the printing system
- **All users** who kept their vintage printers alive!

## 🤝 Contributing

Contributions welcome! Please:

1. Fork the repository
2. Test on your Fedora version/Canon model
3. Document any changes needed
4. Submit a Pull Request

If you test this on other Fedora versions or Canon printer models, please share your results!

## 🔗 Related Projects

- [CUPS](https://github.com/OpenPrinting/cups) - Common Unix Printing System
- [OpenPrinting](https://openprinting.github.io/) - Linux printing database
- [Fedora Printing](https://docs.fedoraproject.org/en-US/quick-docs/cups/) - Official Fedora CUPS docs

---

**Made with ❤️ for Fedora Linux users and vintage printer enthusiasts**

**⚠️ Disclaimer**: This is an unofficial driver created through reverse engineering. Use at your own risk. The original Canon proprietary drivers may still be required for some advanced features.

*Found this helpful? Give it a ⭐ on GitHub!*
