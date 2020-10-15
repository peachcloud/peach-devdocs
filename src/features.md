# Features

PeachCloud features can be broadly divided between device management and Scuttlebutt functionality.

The anticipated features for the MVP release of PeachCloud are listed below. The majority of these features will be exposed via the web interface and some will also be exposed via the physical interface.

Since PeachCloud is built on a JSON-RPC microservices architecture, the APIs of the underlying system should be relatively simple for developers to interface with and extend. Please visit the documentation for individual microservices for further information.

_Note: This is a work-in-progress. Expect changes._

-----

## Scuttlebutt Features

- Profile
  - Display avatar
  - Set avatar (upload file)
  - Display bio
  - Update bio
- Peers
  - List friends
  - List followers
  - List follows
  - List locally-connected peers
  - Follow
  - Unfollow
  - Block
  - Mute (private block)
- Invites
  - Create an invite
    - Text-based (hash)
  - Share an invite
    - Send to a peer within SSB (private message)
    - Share publically within SSB (public post)
    - Send via email
  - Accept an invite
  - Monitor an invite
    - Check if the invite has been accepted
    - For multi-use invites, show number of used & unused invite-slots
  - Cancel an invite (_not sure if this is currently possible_)
- Blobs
  - Display size of blob store (disk utilisation)
  - Prune blobs
    - By size
    - By date
    - By author

-----

## Device Features

- Device status
  - Hardware
    - CPU usage
    - Memory usage
    - Storage usage
    - Disk I/O
  - Software
    - Version info of PeachCloud, sbot, plugins
    - Scripts
    - Plugins
  - Network
    - Display network mode (AP or client)
      - If AP, list connected devices
    - Display current connection(s)
      - Ethernet
      - WiFi
    - Display signal strength
    - Display bandwidth usage
    - Display hostname & external IP
    - Display internal IP
  - Logs
    - Display system logs
  - Errors
    - List errors
    - Report a bug / error
      - Via SSB message
      - Via email
- Configuration
  - Access control
    - Change user password
    - Change administrator password
  - Network
    - Set network mode (AP or client)
    - List available networks
    - Connect to a network
    - Disconnect from a network
    - Forget a network
    - Modify a network password
  - Updates
    - Check for available updates
    - Download updates
    - Install / apply update
  - Backups
    - Create backup
      - Secret key
      - Configuration (device settings)
    - Export backup
      - External storage (USB)
    - List backup history
    - Schedule backups
    - Delete previous backups / backup history
  - Alerts
    - Set alerts based on CPU, memory, disk, bandwith-usage thresholds
    - List previously-defined alerts
    - Delete alerts
  - Miscellaneous
    - List current datetime
    - Set datetime
    - Display current timezone
    - Set timezone
- Documentation
  - Browse
    - Scuttlebot
    - Scuttlebutt
    - PeachCloud
  - Search
