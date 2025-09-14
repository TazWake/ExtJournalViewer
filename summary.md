# EXT Journal Analyzer - Code Summary

## Project Overview

The EXT Journal Analyzer is a forensic tool that extracts and analyzes EXT3/4 filesystem journals from  disk images, converting binary journal structures into human-readable CSV format for forensic investigations. It supports both raw disk images (DD format) and Expert Witness Format (EWF) files.

## Core Components

1. Main Application (main.cpp)

   - Command-line interface with argument parsing
   - Orchestrates the workflow: image handling → journal parsing → CSV export
   - Supports options for image type, verbose output, manual journal offsets, partition handling, and
     transaction sequence filtering

2. Image Handler (image_handler.h/cpp)

   - Handles reading from both raw DD and EWF format files
   - Automatically detects image types based on file extensions
   - Locates journals within filesystems by:
     - Reading superblock information
     - Parsing inode 8 (standard journal location)
     - Supporting manual offset specification
     - Searching common journal locations if automatic detection fails
   - Provides methods for reading data blocks at specific offsets

3. Journal Parser (journal_parser.h/cpp)

   - Parses EXT3/4 journal structures (JBD/JBD2)
   - Processes journal transactions consisting of:
     - Descriptor blocks (listing filesystem blocks being updated)
     - Data blocks (actual filesystem data)
     - Commit blocks (marking transaction completion)
     - Revocation and superblock blocks
   - Implements forensic analysis features:
     - Inode structure parsing (file metadata extraction)
     - Directory entry parsing (filename tracking)
     - Block type identification (inode/directory/file data)
     - String analysis in data blocks (extracts readable content)
     - Path reconstruction (builds full file paths from directory entries)
     - Journal mode detection (JOURNAL/ORDERED/WRITEBACK)
     - Forensic summary generation

4. CSV Exporter (csv_exporter.h/cpp)
   - Exports parsed journal data to structured CSV format
   - Includes comprehensive forensic information:
     - Relative timing (T+0, T+1, etc.)
     - Transaction sequence numbers
     - Filesystem block numbers
     - Operation types and affected inodes
     - File paths and data sizes
     - File type classifications and metadata
     - Full reconstructed paths

## Key Features Implemented

   1. Multi-format Support: Reads both raw DD and EWF (.E01/.Ex01) image files
   2. Automatic Journal Detection: Locates journals in EXT3/4 filesystems
   3. Enhanced Forensic Analysis: Extracts metadata and readable strings from journal data
   4. Path Reconstruction: Builds complete file paths from directory entries
   5. Comprehensive Output: Generates detailed CSV reports with forensic metadata
   6. Partition Support: Handles multi-partition images with offset specifications
   7. Transaction Filtering: Supports processing specific transaction sequence ranges

## Technical Details

   - Language: C++17
   - Build System: CMake
   - Dependencies: libewf for EWF format support
   - Endianness Handling: Properly converts big-endian journal data to host byte order
   - Error Handling: Comprehensive error checking with informative messages
   - Memory Management: Uses smart pointers and RAII principles

##  Current Status

The codebase implements a complete forensic journal analysis tool with all core functionality:

   - Image handling for multiple formats
   - Journal parsing with metadata extraction
   - String analysis in data blocks
   - Path reconstruction
   - CSV export with forensic information
   - Forensic summary generation

  The implementation follows the planned phases with full support for inode analysis, directory operations
  detection, and path resolution as outlined in the development plan.
