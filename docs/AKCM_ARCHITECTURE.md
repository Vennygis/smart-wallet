# AKCM — Associated Keywords Character Model

Version: 0.1, 2026-10-14. Design specification; not an operating-system kernel or deployed IPNS service.

## Purpose
AKCM is a typed, provenance-aware association graph connecting characters, keywords, definitions, countermeanings, translations, actions, devices, files, blockchain references and visual artifacts. KAT means Keywords Associated Together. The period (.) is an independent database operator, not part of KAT.

## Layer contract
1. Input: UTF-8 text, structured metadata, signed device events and authorized content references.
2. Lexical: Unicode code point, normalized token, consonant index, language, morphology, dictionary source, and original spelling.
3. Semantic: typed edges (defines, translates, acts_on, opposes, derived_from, references) with confidence, timestamp, provenance and human override.
4. Bionary: experimental 38-bit and 48-bit association identifiers, explicitly versioned; identifiers are not encryption, universal semantic truths, or lossless Unicode encodings.
5. Boundary: 127 is the maximum positive signed 7-bit integer / ASCII DEL code; 128 is the first value outside 7-bit unsigned ASCII. For AKCM, a 127-bit metadata payload and a 128-bit state representation are separate, explicitly defined application formats, not CPU modes. Never silently truncate a 128-bit value to 127 bits.
6. Execution: authorization/policy gate, verified target chain or local process, signed intent, audit receipt, rollback or compensation where possible. Never infer execution permission from a word association.
7. Storage: transactional local database plus content-addressed IPFS objects, with IPNS pointers for mutable, signed publication references.
8. Visuals: deterministic SVG/vector scenes and graph views indexed by CID; semantic edges may refer to regions, glyphs, # anchors, and visual versions.

## Storage schema (logical)
- entities(id UUID, kind, canonical_name, namespace, created_at)
- lexical_forms(id, entity_id, locale, original, normalized, consonant_signature, unicode_codepoints)
- associations(id, source_id, predicate, target_id, confidence, provenance_cid, revision)
- bit_mappings(id, entity_id, format_version, bit_width, value_hex, encoding_method, checksum)
- artifacts(id, entity_id, mime_type, cid, byte_length, visibility, encryption_envelope_ref)
- ipns_records(id, name, current_cid, sequence, signature_ref, updated_at)
- executions(id, intent_hash, principal_id, authorization_ref, environment, status, receipt_cid)
- os_targets(id, architecture, abi, endian, feature_flags, adapter_version)

## x86 / x86-64 compatibility
Use portable user-space Rust/C/Python services and SQLite/PostgreSQL; compile distinct 32-bit x86 and 64-bit x86-64 binaries only where toolchains and dependencies support them. A 127/128-bit logical mapping does not change the machine word size or make a new OS. Define serialization with fixed-width byte arrays, explicit endianness, schema versions and test vectors. Kernel/bootloader work is a separate project.

## IPFS / IPNS publishing
Serialize versioned canonical JSON (or DAG-CBOR), hash/pin encrypted private artifacts and public metadata separately, save CIDs in the database, then publish an authorized signed IPNS update referencing a manifest CID. Keep prior CIDs for provenance. Visual manifest fields: schema_version, graph_cid, svg_cid, thumbnail_cid, root_entity, revision, timestamp, signatures. Pinata gateway for public reads: https://red-gentle-buzzard.mypinata.cloud (gateway only, never credentials). IPNS writes require an authenticated publisher; a gateway hostname does not grant publishing rights.

## Integration
AKCM -> KAT association graph -> Bionary mapping -> policy/authorization -> adapter (local/testnet/verified mainnet) -> immutable audit receipt. Integrations: GAEA, ALOTOTAX # glyph anchors, Æther Vault, e00198, GCO, IPFS/Pinata, and authorized wallet interfaces. RPC endpoints must be verified; do not introduce proxy RPCs. Existing repository files remain unchanged except this additive document.

## Acceptance tests
Round-trip serialization for 127- and 128-bit values; x86/x86-64 parity; Unicode normalization and collision handling; multilingual association provenance; deterministic CID generation; IPNS signature verification and sequence monotonicity; encrypted-content access control; visual CID consistency; deny-by-default execution authorization; no secrets in public manifests.

## Security
Never store wallet seed phrases, raw fingerprints, private keys, API tokens or unencrypted sensitive chat archives in IPFS, IPNS, public repos or manifests. Use OS-backed key storage and explicit user authorization for signing and publishing. IPFS is public by default and IPNS is a mutable naming layer, not a database.
