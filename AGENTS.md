# Codex instructions

## Using remove-ai-marks

Whenever you need to use the remove-ai-marks skill in this repository:

1. Before processing any files, check `http://127.0.0.1:8765/health`.
2. If the service is unavailable, start it from the repository root in the
   background, bound only to `127.0.0.1` on port `8765`, using:

   ```bash
   nohup python3 service/scripts/server.py --host 127.0.0.1 --port 8765 > /tmp/watermarks-service.log 2>&1 < /dev/null &
   ```

3. Retry the health check and confirm HTTP `200` before using the skill. For
   example:

   ```bash
   curl --silent --show-error --fail --max-time 10 \
     --output /dev/null --write-out '%{http_code}\n' \
     http://127.0.0.1:8765/health
   ```

   Allow a short, bounded retry period for startup. If health cannot be
   confirmed, report the failure and relevant errors from
   `/tmp/watermarks-service.log`; do not process files.
4. Inspect each file through the service before cleaning. Preserve originals
   and write separate cleaned copies, using `*.cleaned.*` names by default.
5. Check the service's `/capabilities` endpoint before relying on optional
   features. Report unavailable dependencies and unsupported operations rather
   than claiming those features work.
6. Never enable public internet access, expose the service publicly, or add
   credentials without the user's explicit permission.
