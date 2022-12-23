<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/stored-cross-site-scripting/main/stored-cross-site-scripting.png"></p>

A threat actor may inject malicious content into a vulnerable target.

## Example #1
1. Threat actor infects a vulnerable target with malicious code
2. Bob requests the vulnerable target and receives the malicious code
3. When malicious code gets executed, it calls back the threat actor
 
## Impact
Vary

## Risk
- Read & modify data

## Redemption
- Output encoding
- Browser built-in XSS preveiton

## ID
cb251c97-067d-4f13-8195-4f918273f41b

## References
- [wiki](https://en.wikipedia.org/wiki/cross-site_scripting)
