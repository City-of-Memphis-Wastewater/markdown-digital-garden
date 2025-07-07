---
{"dg-publish":true,"permalink":"/ai-generated/fixing-hard-to-read-text-colors-in-windows-power-shell/","noteIcon":"","created":"2025-07-07T14:23:43.876-05:00"}
---

Date: [[-Daily Activity Log-/2025 05-May 27\|2025 05-May 27]]

# Fixing Hard-to-Read Text Colors in Windows PowerShell

If you're experiencing hard-to-read or low-contrast text in PowerShell, especially when using custom themes or syntax highlighting, you can resolve it without installing modules, changing your PowerShell version, or using admin rights.

## Solution: Enable Automatic Text Contrast Adjustment

### Steps

1. Open a PowerShell window.
2. Right-click the window title bar and select **Properties**.
3. Navigate to the **Colors** tab.
4. Scroll to the bottom of the dialog.
5. Set **“Automatically adjust lightness of indistinguishable text”** to **Always**.
6. Click **OK** to save and close the dialog.

### Why This Works

This setting forces PowerShell to automatically improve contrast when background and foreground colors are too similar, making the text easier to read.

### Notes

- This setting applies immediately.
- No restart or configuration file changes required.
- Works for both legacy and modern Windows terminals that still use the classic Properties dialog.

