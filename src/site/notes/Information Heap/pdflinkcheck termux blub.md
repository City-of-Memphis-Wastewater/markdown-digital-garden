---
{"dg-publish":true,"permalink":"/information-heap/pdflinkcheck-termux-blub/","noteIcon":"","created":"2025-12-18T17:00:03.861-06:00"}
---

Date: [[-Daily Activity Log-/2025 12-December 18\|2025 12-December 18]]

#### Termux Compatibility as a Key Goal

A key goal of City-of-Memphis-Wastewater is to release all software as Termux-compatible. While using PyMuPDF as a Python package with dependency resolution on Termux simply isn't possible, we are proud to have achieved a work around! Now, there is PDF Engine selection in both the CLI and the GUI. The default in all code functions is set to `pypdf` - PyMuPDF can be explicitly requested.

We tried alternative PDF libaries like `pdfminer`, `pdfplumber`, and `borb`, but none of these offered the level of detail concerning GoTo links.

Due to Termux compatibility goals, we do not generally make Tkinter-based interfaces, so that was a fun, minimalist opportunity on this project.

  

Termux compatibility is important in the modern age as Android devices are common among technicians, field engineers, and maintenace staff.

Android is the most common operating system in the Global South.

We aim to produce stable software that can do the most possible good.

  

We love web-stack GUIs served locally as a final product.

All that packaged up into a Termux-compatible ELF or PYZ - What could be better!

  

In the future we may find a work-around and be able to drop the PyMuPDF dependency.

This would have lots of implications:

- Reduced artifact size.

- Alpine-compatible Docker image.

- Web-stack GUI rather than Tkinter, to be compatible with Termux.

- A different license from the AGPL3, if we choose at that time.

  

In the meantime, the standalone binaries and pipx installation provide excellent cross-platform support on Windows, macOS, and standard Linux desktops/laptops.