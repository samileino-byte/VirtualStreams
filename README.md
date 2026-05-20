Stream Groups:
- System streams (1-6): Reserved, shorter timeouts, 2 retries
- Application streams (7+): User/app data, longer timeouts, 3 retries

Security Modes:
- EPHEMERAL: Socket open > send > close per transfer (no persistent connections, robust against TCP interception)
- PERSISTENT: Keep-alive connections for high-throughput scenarios
- TUNNEL_VPN: Routes through TunnelManager (IKEv2/SSH/GRU), still ephemeral socket lifecycle within the tunnel

Protocol Support:
- TCP (SOCK_STREAM)
- UDP (SOCK_DGRAM)
- TLS_TCP (TLS over TCP for VPN streams)

Key Classes:
- VirtualStream: Core stream manager (open, close, send, listen, stats)
- VirtualStreamPipe: Forwards data between two streams (system-to-app piping)
- StreamConfig: Full config (protocol, security mode, timeouts, retries, tunnel ID)
- StreamTransfer: Per-transfer result with socket open/close counts

Integrates with existing modules:
- WinsockEngine (socket lifecycle, linger, reuse, nodelay, keepalive)
- TunnelManager (VPN tunnel routing)
- PortSwitcher (port switching)
- Logger (event logging)
- ConnectionPool (optional pooling)

Tests cover:
- Stream ID validation (1-6 system, 7+ app)
- Lifecycle (open/close/reopen/duplicate)
- Concurrent thread safety
- Ephemeral socket open/close per transfer
- All protocol types (TCP/UDP/TLS)
- All security modes (ephemeral/persistent/VPN)
- Event callbacks and stats tracking
- Pipe creation between streams
- Full scenario (6 system + 4 app streams simultaneously)

Updated CMakeLists.txt bumped to v1.2.0 with the new module.
