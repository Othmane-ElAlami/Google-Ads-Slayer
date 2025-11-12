# Google Ads Slayer Hosts File

A minimal, targeted hosts file that blocks Google's ad-serving infrastructure while preserving core Google services functionality.

## What This Does

This hosts file redirects Google's primary ad-serving domains to `0.0.0.0`, effectively blocking Google ads across websites, YouTube (partially), and mobile apps. Unlike comprehensive ad blockers, this approach is surgical - it only targets Google's advertising infrastructure.

### Key Features

- **Minimal Impact**: Only blocks ad-serving domains, leaving search, Gmail, Drive, and other Google services intact
- **Comprehensive Coverage**: Blocks DoubleClick, AdSense, AdMob, YouTube ads, and related tracking domains
- **Cross-Platform**: Works on Windows, macOS, Linux, and rooted/jailbroken mobile devices
- **Easy Maintenance**: Simple text file format, easy to update and customize
- **Zero Performance Overhead**: No browser extension required, works at the OS level

## Installation

### Windows

1. Download the `hosts` file from this repository
2. **Backup your existing hosts file**: Copy `C:\Windows\System32\drivers\etc\hosts` to `hosts.backup`
3. Open Notepad as Administrator (right-click → Run as administrator)
4. Open `C:\Windows\System32\drivers\etc\hosts`
5. Append the contents of the downloaded hosts file (or replace if starting fresh)
6. Save the file
7. Flush DNS cache: Open Command Prompt as Administrator and run:

   ```cmd
   ipconfig /flushdns
   ```

### macOS

1. Download the `hosts` file from this repository
2. Open Terminal
3. Backup your existing hosts file:

   ```bash
   sudo cp /etc/hosts /etc/hosts.backup
   ```

4. Edit the hosts file:

   ```bash
   sudo nano /etc/hosts
   ```

5. Append the downloaded content (or use `sudo cat downloaded-hosts >> /etc/hosts`)
6. Save and exit (Ctrl+O, Enter, Ctrl+X in nano)
7. Flush DNS cache:

   ```bash
   sudo dscacheutil -flushcache; sudo killall -HUP mDNSResponder
   ```

### Linux

1. Download the `hosts` file from this repository
2. Open Terminal
3. Backup your existing hosts file:

   ```bash
   sudo cp /etc/hosts /etc/hosts.backup
   ```

4. Edit the hosts file:

   ```bash
   sudo nano /etc/hosts
   ```

5. Append the downloaded content
6. Save and exit
7. Flush DNS cache (varies by distribution):

   ```bash
   # Ubuntu/Debian with systemd-resolved
   sudo systemd-resolve --flush-caches
   
   # Systems with nscd
   sudo systemctl restart nscd
   
   # Systems with dnsmasq
   sudo systemctl restart dnsmasq
   ```

### Mobile Devices

**Important**: Standard mobile devices do **not** allow hosts file modification without root/jailbreak access.

- **Android**: Requires root access. Use apps like AdAway or use a DNS-based solution like Blokada
- **iOS**: Requires jailbreak or use DNS-based solutions like AdGuard DNS (no jailbreak required)

**Recommended alternative**: Use DNS-level blocking (Pi-hole, AdGuard Home) or VPN-based ad blockers that don't require root/jailbreak.

## What Gets Blocked

This hosts file blocks the following Google advertising domains:

### Core Ad Infrastructure

- **DoubleClick**: `doubleclick.net`, `ad.doubleclick.net`, `googleads.g.doubleclick.net`, `securepubads.g.doubleclick.net`
- **AdSense**: `googlesyndication.com`, `pagead2.googlesyndication.com`
- **AdWords/Google Ads**: `googleadservices.com`, `pagead2.googleadservices.com`
- **Tag Services**: `googletagservices.com`

### Content Delivery Networks

- **2MDN** (DoubleClick CDN): `2mdn.net`, `s0.2mdn.net`, `static.2mdn.net`, `m1.2mdn.net`

### Platform-Specific

- **YouTube Ads**: `ads.youtube.com` (partial blocking - see limitations)
- **Mobile Ads**: `mobileads.google.com`, `media.admob.com`
- **Generic Ad Endpoints**: `adservice.google.com`, `ads.google.com`

## What Still Works

This blocklist is designed to be surgical. The following Google services remain **fully functional**:

✅ Google Search (search results work, but ad clicks are blocked)  
✅ Gmail  
✅ Google Drive  
✅ Google Maps  
✅ Google Docs/Sheets/Slides  
✅ Android app updates  
✅ Google Play Store  
✅ YouTube video playback  
✅ Google Fonts, APIs, reCAPTCHA  
✅ Google Analytics (intentionally not blocked)  
✅ Google OAuth/Login  

## Important Limitations

⚠️ **YouTube Ad Blocking**: This hosts file provides **partial YouTube ad blocking only**. Modern YouTube serves many ads from the same domains as video content, making host-level blocking ineffective. For comprehensive YouTube ad blocking, use browser extensions like uBlock Origin or Sponsor Block.

⚠️ **Visual Gaps**: Websites may show empty spaces where ads would have loaded. This is normal behavior.

⚠️ **Google Search Ads**: Text ads on Google Search will appear, but clicking them will fail (the click-tracking domain is blocked).

⚠️ **Some Mobile Apps**: Apps with embedded Google ads may show errors or blank spaces.

## Testing

### Verify Blocking is Active

1. Open Command Prompt/Terminal and test:

   ```bash
   ping doubleclick.net
   ```

   Should return `0.0.0.0` or fail to resolve

2. Visit a website with Google AdSense ads (most news sites) - you should see blank spaces where ads would appear

3. Watch a YouTube video - some ads may be blocked, but not all

### Verify Google Services Work

- Search on Google.com - should work normally
- Access Gmail - should load completely
- Open Google Drive - should function properly

## Troubleshooting

### Ads Still Showing?

1. **Flush DNS cache** (see installation steps above)
2. **Clear browser cache**: Some browsers cache DNS lookups aggressively
3. **Try incognito/private mode**: Rules out browser caching
4. **Verify hosts file**: Run `ping doubleclick.net` - should return `0.0.0.0` or fail
5. **Check file syntax**: Ensure no typos, each entry on its own line
6. **Restart browser**: Some browsers require restart to apply DNS changes

### Google Services Broken?

If you experience issues with Google Search, Gmail, Drive, etc.:

1. **Restore from backup**:

   ```bash
   # Windows (Command Prompt as Admin)
   copy C:\Windows\System32\drivers\etc\hosts.backup C:\Windows\System32\drivers\etc\hosts
   
   # macOS/Linux
   sudo cp /etc/hosts.backup /etc/hosts
   ```

2. **Flush DNS again** after restoring

3. **Report the issue**: If a legitimate Google service breaks, please open an issue on GitHub

### "Permission Denied" Errors

- Windows: Must run Notepad/editor as Administrator
- macOS/Linux: Must use `sudo` before commands

### Changes Not Taking Effect?

- Some antivirus software monitors the hosts file - temporarily disable it
- Windows may require a full system restart in some cases
- Verify the file has no `.txt` extension (should be just `hosts`, not `hosts.txt`)

## Customization

The hosts file is a plain text file. You can customize it by:

### Adding More Domains

```hosts
0.0.0.0    additional-ad-domain.com
```

### Temporarily Disabling a Block

Add `#` at the start of a line:

```hosts
# 0.0.0.0    doubleclick.net
```

### Combining with Other Lists

You can merge this with other hosts-based blocklists, but be aware:

- Larger hosts files increase DNS lookup time slightly
- Conflicts may break services
- Always keep a backup

## Performance Impact

**Negligible**. Hosts file blocking is extremely efficient:

- No browser overhead
- No memory usage
- Instant DNS resolution (0.0.0.0 is immediate)
- Works even if your browser crashes

## Privacy & Security

✅ **Pros**:

- Blocks ad tracking at the OS level
- Works in all browsers and apps
- No data sent to third-party filter lists
- No extension permissions needed

❌ **Cons**:

- Doesn't block all tracking (Google Analytics intentionally preserved)
- Doesn't provide HTTPS filtering
- Can't use advanced filter rules like browser extensions

## Updating

Google occasionally adds new ad-serving domains. To update:

1. Check this repository for updates
2. Download the new hosts file
3. Follow the installation steps again (your backup will remain intact)
4. Flush DNS cache

**Recommended**: Check for updates monthly or subscribe to repository releases.

## Comparison with Alternatives

| Solution | Pros | Cons |
|----------|------|------|
| **Hosts file** | OS-level, no overhead, works everywhere | Limited rules, can't block YouTube fully |
| **uBlock Origin** | Advanced filtering, YouTube blocking | Browser-only, requires extension |
| **Pi-hole** | Network-wide blocking | Requires separate hardware/server |
| **DNS filtering** | Works on mobile without root | Requires VPN/DNS change, slower |

**Best approach**: Use this hosts file + uBlock Origin for maximum coverage.

## Disclaimer

**Use at your own risk.** Modifying your system's hosts file can:

- Break websites or services if domains are incorrectly blocked
- Require administrative/root access to your system
- Cause unexpected behavior in some applications

This blocklist is provided **as-is with no warranty**. The maintainers are not responsible for:

- Service disruptions
- Data loss
- System instability
- Any other issues arising from use

**Always maintain a backup** of your original hosts file.

## Contributing

Contributions are welcome! Please:

1. **Test thoroughly** on multiple platforms before submitting
2. **Document your changes** - explain what domains you're adding and why
3. **Verify domains are ad-serving only** - don't break legitimate services
4. **Update this README** if adding significant functionality
5. **Submit a pull request** with clear descriptions

### Reporting Issues

If you find:

- A Google service that breaks
- An ad domain that's not blocked
- An error in the documentation

Please open a GitHub issue with:

- Your operating system
- Steps to reproduce
- Expected vs. actual behavior

## Related Projects

- **[StevenBlack/hosts](https://github.com/StevenBlack/hosts)** - Comprehensive unified hosts file (much larger)
- **[uBlock Origin](https://github.com/gorhill/uBlock)** - Advanced browser-based content blocker
- **[AdAway](https://github.com/AdAway/AdAway)** - Ad blocker for Android using the hosts file and local VPN
- **[Pi-hole](https://github.com/pi-hole/pi-hole)** - Network-wide DNS-based ad blocking

## License

This project is licensed under the 0BSD License. See the [LICENSE](LICENSE) file for details.

## Support

- **Issues**: [GitHub Issues](https://github.com/Othmane-ElAlami/Google-Ads-Slayer/issues)
- **Discussions**: [GitHub Discussions](https://github.com/Othmane-ElAlami/Google-Ads-Slayer/discussions)
- **Updates**: Watch this repository for updates

---

**Star this repository** if it helps you block ads! ⭐️
