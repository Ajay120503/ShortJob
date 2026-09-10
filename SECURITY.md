# Security and reliability changes

## September 2026

- Socket connections require a valid access token and an active, unblocked account. The server derives the user identity from that token.
- Private chat room subscriptions require conversation membership. Typing events use the authenticated identity; read receipts originate from the HTTP controller after persistence.
- Expired socket sessions disconnect. The client uses the existing HTTP refresh flow and restores conversation subscriptions after reconnecting.
- Post image removal accepts only image IDs already attached to the edited post. Removed assets are deleted after the post is saved. Updates reject excess images rather than silently discarding them.
- Remote OCR downloads require HTTPS on `res.cloudinary.com`, reject redirects and credentials, and enforce the 6 MiB limit while streaming, including responses without a content-length header.
- Production error responses hide unexpected internal exception messages.

Run `npm test` from `server` for security regression checks. The checks use mocked database access and downloads; they do not modify application data.

Deploy the client and server changes together because chat now requires token authentication during the socket handshake. Custom media CDNs are not accepted by remote OCR unless separately reviewed and supported.

These are targeted fixes, not a complete security audit. Existing access tokens and their storage strategy, refresh-token revocation, upload content inspection, and broader application authorization should be reviewed separately.
