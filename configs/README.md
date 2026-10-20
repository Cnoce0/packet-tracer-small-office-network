# Device configuration files

- `R1.txt` is the final intended configuration for the Cisco 2911 router.
- `SW1.txt` is the final intended configuration for the Cisco 2960 access switch.
- `server-setup.md` configures the Packet Tracer server through its GUI because Server-PT does not use IOS CLI syntax.

Apply the switch and router configurations to the matching model and interfaces in the build guide. IOS support varies by Packet Tracer release. The configurations request 1024-bit RSA keys; if Packet Tracer rejects that exact line, use the interactive `crypto key generate rsa` syntax described in `docs/packet-tracer-build-guide.md`.

The `netadmin` and console credentials in the config files are example lab credentials only. They are deliberately documented as public placeholders. Do not reuse them on real equipment.
