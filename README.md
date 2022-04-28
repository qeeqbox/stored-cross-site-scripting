<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/stored-cross-site-scripting/main/stored-cross-site-scripting.png"></p>

An adversary may inject malicious content into a vulnerable target.

## Example #1
1. Adversary infects a vulnerable target with malicious code
2. Bob requests the vulnerable target and receives the malicious code
3. When malicious code gets executed, it calls back the Adversary
 
## Impact
Vary

## Risk
- read & modify data

## Redemption
- output encoding
- browser built-in XSS preveiton

## ID
cb251c97-067d-4f13-8195-4f918273f41b

## References
- [wiki](https://en.wikipedia.org/wiki/cross-site_scripting)
