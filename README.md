Python tools for working with the [_Creatures_](https://creatures.wiki/) series of games.

## File types

File types supported:
- SPR images (read+write, including back.spr)
- S16 images (read+write, including back.s16)
- C16 images (read+write)
- BLK images (read+write, macOS big-endian version not supported)
- Creatures 1 COB/RCB files (read only)
- GEN files (read only, version 3 only)
- PRAY files (AGENTS, CREATURE, FAMILY, SEAMONKEYS, etc) (read only)
- PRAY source files (read+write)

## Command-line Tools

This library also comes with a number of useful tools. Each tool lives under
the `creaturestools/bin/` directory and can be run like `python creaturestools/bin/$toolname.py`.

Some (eventually, all) of the tools are also configured as entry points, which
means you can `pip install` this repository and then use them immediately, e.g.:

```bash
pip install git+https://github.com/openc2e/pycreaturestools/
spritedumper my_sprite.c16
```

Available tools:
- **cobdumper**: Takes a C1 COB/RCB and writes a CAOS2Cob script and PNG of the thumbnail
- **gen2json**: Parses a GEN file and outputs a JSON representation
- **praycrush**: Recompresses a PRAY file to make it as small as possible
- **praydumper**: Takes a PRAY file (.AGENTS, .FAMILY, .SEAMONKEYS, etc) and decompiles it
- **spritedumper**: Takes a sprite (SPR, S16, C16, or BLK) and writes out the frames as PNGs
- caossyntaxdumper: Parses a caos.syntax file
- creaturescavesdownloader: Downloads files from Creatures Caves
- creaturescavesextractor: Extracts files downloaded from Creatures Caves
- extract_alb2203_sprs: 👀
- glstdumper: Parses a GLST block
- gno_dumper: Parses genome notes (GNO) files
- new_breed_installer_extract: Extracts files created by Kinnison's New Breed Installer
- parse_2er: Parses 2ER files
- parse_creaturesarchive: Parses CreaturesArchive files (poorly)
- read_aphro: Parses Aphro, AphroBU, Health, and HealthBU files
- read_pefile: Parses resources from Windows executables
- sfcdumper: Parses SFC files
- sfcdumperv2: Parses SFC files
- szdd_dumper: Decompresses files compressed on old versions of MS-DOS
- translate_voice: Converts a string of text into Creatures sounds

## Library API

**creaturestools.caos**
- `format_c1_caos(str) -> str`

**creaturestools.cobs**
- `read_cob1_file(fname_or_stream) -> creaturestools.cobs.Cob1File`

**creaturestools.genetics**
- `read_gen3_file(fname_or_stream) -> List[creaturestools.genetics.Gene]`
- `svrule3_from_bytes(bytes[48]) -> List[str]`
- `SVRULE3_OPCODES: Mapping[int, str]`
- `SVRULE3_OPERAND_TYPES: Mapping[int, str]`

**creaturestools.pray**
- `read_pray_file(fname_or_stream) -> List[(block_name, block_type, data)]` (block data is either `bytes` or a `dict`)
- `write_pray_file(fname_or_stream, blocks, compression=9)`
- `PRAY_TAG_BLOCK_TYPES: List[str]`

**creaturestools.praysource**
- `pray_to_pray_source(blocks, Optional[filenamefilter]) -> str`
- `parse_pray_source(pray_source) -> List[(block_name, block_type, data)]` (block data is either a `dict` or `pathlib.Path`, dict values are either `int`, `str`, or `pathlib.Path`)
- `parse_pray_source_file(fname_or_stream) -> List[(block_name, block_type, data)]` (block data is either a `dict` or `pathlib.Path`)
- `pray_load_file_references(blocks, fileloaderfunc) -> List[(block_name, block_type, data)]` (replaces all `pathlib.Path` values with the result of fileloaderfunc, resulting block data is either `bytes` or a `dict`)

**creaturestools.sprites**
- `read_spr_file(fname_or_stream) -> List[PIL.Image]`
- `read_s16_file(fname_or_stream) -> List[PIL.Image]`
- `read_c16_file(fname_or_stream) -> List[PIL.Image]`
- `read_blk_file(fname_or_stream) -> PIL.Image`
- `write_s16_file(fname_or_stream, List[PIL.Image])`
- `write_c16_file(fname_or_stream, List[PIL.Image])`
- `write_spr_file(fname_or_stream, List[PIL.Image])`
- `write_blk_file(fname_or_stream, PIL.Image)`
- `stitch_to_sheet(List[PIL.Image]) -> PIL.Image`
- `CREATURES1_PALETTE: int[256 * 3]`