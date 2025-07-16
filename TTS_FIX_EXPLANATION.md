# ElevenLabs TTS "Invalid URL" Fix

## Problem
ElevenLabs Text-to-Speech was failing with "Invalid URL" error when trying to generate speech.

## Root Cause
The `.env` file contained Windows-style line endings (CRLF - `\r\n`) instead of Unix-style line endings (LF - `\n`). 

When the Docker container (Linux-based) read the environment variables, it included the carriage return character (`\r`) as part of the API key value, making it:
```
TTS_API_KEY=sk_0f7285...c83b\r
```

This invisible `\r` character in the HTTP header caused axios to throw an "Invalid URL" error because HTTP headers cannot contain carriage return characters.

## Environment Details
- Host OS: Windows
- Development Environment: WSL2 Debian 12
- IDE: Cursor
- Application: LibreChat running in Docker containers

## Solution
Remove carriage returns from the `.env` file:
```bash
sed -i 's/\r$//' .env
```

## Prevention
To prevent this issue in the future when working with WSL:

1. Configure your IDE (Cursor/VS Code) to use LF line endings for `.env` files
2. Add a `.gitattributes` file with:
   ```
   .env text eol=lf
   *.env text eol=lf
   ```
3. Use `dos2unix` command on Windows-created files before using them in WSL/Docker

## Additional Notes
- The actual ElevenLabs API key was valid
- Once line endings were fixed, a secondary issue was discovered: ElevenLabs subscription had a payment issue
- After resolving both issues, TTS worked perfectly

## Code Changes Made
- Added voice validation in `client/src/hooks/Input/useTextToSpeechExternal.ts` to prevent empty voice parameters
- No other code changes were necessary - the issue was purely configuration-related