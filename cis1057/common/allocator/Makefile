# Makefile for Allocator component
# Build system for common memory allocator

CC = gcc
CFLAGS = -Wall -Wextra -std=c99 -pedantic -g
INCLUDES = -Iinclude -I../

# Detect OS for cross-platform commands
ifeq ($(OS),Windows_NT)
    MKDIR = if not exist "$@" mkdir "$@"
    RMDIR = if exist "$(1)" rmdir /s /q "$(1)"
    RM = del /q
else
    MKDIR = mkdir -p $@
    RMDIR = rm -rf $(1)
    RM = rm -f
endif

# Source and object directories
SRCDIR = src
TESTDIR = test
OBJDIR = obj
BINDIR = bin

# Source files (only Allocator for now)
SOURCES = $(SRCDIR)/Allocator.c
OBJECTS = $(SOURCES:$(SRCDIR)/%.c=$(OBJDIR)/%.o)

# Test files
TEST_SOURCES = $(wildcard $(TESTDIR)/*.c)
TEST_BINARIES = $(TEST_SOURCES:$(TESTDIR)/%.c=$(BINDIR)/%)

# Default target
all: $(TEST_BINARIES)

# Create directories
$(OBJDIR) $(BINDIR):
	$(MKDIR)

# Compile source files to objects
$(OBJDIR)/%.o: $(SRCDIR)/%.c | $(OBJDIR)
	$(CC) $(CFLAGS) $(INCLUDES) -c $< -o $@

# Build test executables
$(BINDIR)/%: $(TESTDIR)/%.c $(OBJECTS) | $(BINDIR)
	$(CC) $(CFLAGS) $(INCLUDES) $< $(OBJECTS) -o $@

# Run specific tests
test-allocator: $(BINDIR)/test_allocator
	./$(BINDIR)/test_allocator

# Run all tests
test: $(TEST_BINARIES)
	@for test in $(TEST_BINARIES); do \
		echo "Running $$test..."; \
		./$$test; \
		echo; \
	done

# Clean build artifacts
clean:
	$(call RMDIR,$(OBJDIR))
	$(call RMDIR,$(BINDIR))

# Phony targets
.PHONY: all test test-allocator clean

# Dependencies
$(BINDIR)/test_allocator: $(OBJDIR)/Allocator.o