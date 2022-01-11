Python tools for working with the [_Creatures_](https://creatures.wiki/) series of games.

## File types

File types supported:
- Creatures 0 SPR images (read only, including 0–127.SPR)
- SPR images (read+write, including back.spr)
- S16 images (read+write, including back.s16)
- C16 images (read+write)
- BLK images (read+write, macOS big-endian version not supported)
- Spritesheets (read+write)
- Creatures 1 COB/RCB files (read only)
- GEN files (read only, version 3 only)
- PRAY files (AGENTS, CREATURE, FAMILY, SEAMONKEYS, etc) (read+write)
- PRAY source files (read+write)
- CreaturesArchive compression wrapper (read only)
- NetBabel (read only, most message types except NET: WRIT)
- PCAP files (read only, used for reading NetBabel packet captures)

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
- **babeldump**: Parses a PCAP file containing NetBabel messages
- **cobdumper**: Takes a C1 COB/RCB and writes a CAOS2Cob script and PNG of the thumbnail
- **gen2json**: Parses a GEN file and outputs a JSON representation
- **praybuilder**: Parse a PRAY source file and writes an AGENTS file
- **praycrush**: Recompresses a PRAY file to make it as small as possible
- **praydumper**: Takes a PRAY file (.AGENTS, .FAMILY, .SEAMONKEYS, etc) and decompiles it
- **spritebuilder**: Takes a spritesheet image and converts it into a C16 sprite file
- **spritecutter**: Takes a spritesheet image and writes out the individual sprites as PNGs
- **spritedumper**: Takes a sprite (SPR, S16, C16, BLK, or "Creatures 0" SPR) and writes out the frames as PNGs
- babeldump: Parses a .pcap file with NetBabel messages
- caossyntaxdumper: Parses a caos.syntax file
- creaturescavesdownloader: Downloads files from Creatures Caves
- creaturescavesextractor: Extracts files downloaded from Creatures Caves
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

**creaturestools.babel**
- `parse_netbabel_client_message(f) -> NetBabelClientMessage`
- `parse_netbabel_server_message(f) -> NetBabelServerMessage`
- `parse_netbabel_header(f) -> NetBabelHeader`
- `parse_netbabel_server9_pray(f, header) -> NetBabelServer9PrayMessage`
- `parse_netbabel_server10_net_line_response(f, header) -> NetBabelServer10NetLineResponse`
- `parse_netbabel_server13(f, header) -> NetBabelServer13Message`
- `parse_netbabel_server14(f, header) -> NetBabelServer14Message`
- `parse_netbabel_server15_net_unik_response(f, header) -> NetBabelServer15UnikResponse`
- `parse_netbabel_server19_net_ulin_response(f, header) -> NetBabelServer19UlinResponse`
- `parse_netbabel_server24_net_stat_response(f, header) -> NetBabelServer24StatResponse`
- `parse_netbabel_server545_net_ruso_response(f, header) -> NetBabelServer545RusoResponse`
- `parse_netbabel_server801_creature_history_response(f, header) -> NetBabelServer801CreatureHistoryResponse`
- `parse_netbabel_client9_pray(f, header) -> NetBabelClient9PrayMessage`
- `parse_netbabel_client15_net_unik_request(f, header) -> NetBabelClient15NetUnikRequest`
- `parse_netbabel_client16(f, header) -> NetBabelClient16Message`
- `parse_netbabel_client19_net_ulin_request(f, header) -> NetBabelClient19UlinRequest`
- `parse_netbabel_client24_net_stat_request(f, header) -> NetBabelClient24StatRequest`
- `parse_netbabel_client37_net_line_request(f, header) -> NetBabelClient37NetLineRequest`
- `parse_netbabel_client545_net_ruso_request(f, header) -> NetBabelClient545RusoRequest`
- `parse_netbabel_client801_creature_history(r, header) -> NetBabelClient801CreatureHistory`

**creaturestools.caos**
- `format_c1_caos(str) -> str`

**creaturestools.cobs**
- `read_cob1_file(fname_or_stream) -> creaturestools.cobs.Cob1File`

**creaturestools.creaturesarchive**
- `decompress_creaturesarchive_compressed_file(fname_or_stream) -> bytes`

**creaturestools.genetics**
- `read_gen3_file(fname_or_stream) -> List[creaturestools.genetics.Gene]`
- `svrule3_from_bytes(bytes[48]) -> List[str]`
- `SVRULE3_OPCODES: Mapping[int, str]`
- `SVRULE3_OPERAND_TYPES: Mapping[int, str]`

**creaturestools.pcaps**
- `read_pcap_file(fname_or_stream) -> Dict[(source_addr, dest_addr): List[(timestamp, tcp_data)]]`
- `TimestampedReader(List[(timestamp, tcp_data)]]) -> TimestampedReader`

**creaturestools.pray**
- `read_pray_file(fname_or_stream) -> List[(block_name, block_type, data)]` (block data is either `bytes` or a `dict[str, Union[int: str]]`)
- `write_pray_file(fname_or_stream, blocks, compression=9)`
- `PRAY_TAG_BLOCK_TYPES: List[str]`

**creaturestools.praysource**
- `parse_pray_source_file(fname_or_stream) -> List[(block_name, block_type, data)]` (block data is either a `pathlib.Path` or `dict[str, Union[int: str, pathlib.Path]]`)
- `pray_load_file_references(blocks, fileloaderfunc) -> List[(block_name, block_type, data)]` (replaces all `pathlib.Path` values with the result of fileloaderfunc, resulting block data is either `bytes` or a `dict[str: Union[int, str]]`)
- `generate_pray_source(blocks, Optional[filenamefunc]) -> str`

**creaturestools.sprites**
- `read_palette_dta_file(fname_or_stream) -> PIL.ImagePalette.ImagePalette`
- `read_creatures0_spr_file(fname_or_stream, Optional[palette]) -> List[PIL.Image]`
- `read_spr_file(fname_or_stream, Optional[palette]) -> List[PIL.Image]`
- `read_s16_file(fname_or_stream) -> List[PIL.Image]`
- `read_c16_file(fname_or_stream) -> List[PIL.Image]`
- `read_blk_file(fname_or_stream) -> PIL.Image`
- `write_s16_file(fname_or_stream, List[PIL.Image])`
- `write_c16_file(fname_or_stream, List[PIL.Image])`
- `write_spr_file(fname_or_stream, List[PIL.Image])`
- `write_blk_file(fname_or_stream, PIL.Image)`
- `stitch_to_sheet(images) -> PIL.Image`
- `cut_sheet_to_sprites(image, *, colorkey) -> List[PIL.Image]`
- `find_sprite_sheet_colorkey(image, *, width=5, height=5) -> Optional[colorkey]`
- `is_creatures0_sprite_file(fname_or_stream) -> bool`
- `is_creatures0_sprite_background_piece(images) -> bool`
- `stitch_creatures0_sprite_background(images) -> PIL.Image`
- `CREATURES1_PALETTE: PIL.ImagePalette.ImagePalette`
- `CREATURES0_PALETTE: PIL.ImagePalette.ImagePalette` (from PALETTE.DTA, Creatures 0 has additional palettes 0.PAL–4.PAL)
- `CREATURES0_SPRITE_BACKGROUND_PIECE_NAMES: [str]`