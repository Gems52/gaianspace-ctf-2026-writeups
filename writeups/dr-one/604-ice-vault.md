# 604 · ice-vault

> **Category:** Crypto / Brute-force · **Points:** 125 · **Track:** [DR-ONE](README.md)

## Challenge

The DR-ONE probe confirmed a buried sub-surface ice target but never relayed its location in the clear. The exact coordinates are sealed inside an AES-encrypted `.7z` archive (**headers encrypted too**), locked behind a randomly "mashed" **5-lowercase-letter passphrase**, meant to be cracked with an exhaustive mask attack.

## Approach

The intended route is an exhaustive mask attack on the 7-Zip hash. The search space is only 26⁵ ≈ 11.9 million candidates, which is trivial on a GPU:

```console
$ hashcat -m 11600 -a 3 hash.txt ?l?l?l?l?l
```

**Shortcut used here:** the challenge page itself exposes a free hint (0-point cost) that reveals the passphrase `qvfzk`, which made the brute force unnecessary.

```console
$ 7z x -pqvfzk mission_vault.7z
```

The archive opens straight onto `ice_site.txt`, confirming a sub-surface water-ice deposit at **Utopia Planitia**, planetocentric coordinates **46.7N / 97.3E**. The values are then reformatted to match the required flag template: the designation with no space, followed by the coordinate pair.

## Flag

```
GAIANSPACE{UTOPIA_PLANITIA_46.7N_97.3E}
```

---
[← Back to index](../../README.md)
