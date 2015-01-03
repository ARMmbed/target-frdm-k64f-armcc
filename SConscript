import os

################################################################################
# Macro definitions

# TODO: we could do with a little inheritance here (some of the macros below could come from a
# toolchain-independent module). It might not be worth doing that though, since the common
# part might be fairly small.
# TODO: need to handle different build types (releae/debug), so 'NDEBUG' needs to move
# somewhere else (maybe we need different environments? Check how scons handles release/debug builds)
macros = ['CPU_MK64FN1M0VMD12', 'TARGET_K64F', 'TOOLCHAIN_ARM', 'DTOOLCHAIN_ARM_STD', 'NDEBUG', '__thumb2__']

################################################################################
# Compiler flags (C/C++)

# TODO: need to handle different build types (releae/debug), so '-Os' needs to move
# somewhere else (maybe we need different environments? Check how scons handles release/debug builds)
c_flags = '--cpu=Cortex-M4.fp --split_sections --apcs=interwork --restrict --multibyte_chars -D__thumb2__ --gnu --no_rtti '
cxx_flags = '--no_exceptions --cpp'

################################################################################
# Linker flags

link_script = "ld/MK64F.sct"

################################################################################
# Assembler flags
# scons doesn't automatically add macros and include paths to the assembler invocation, so add them manually

#as_flags = '-mcpu=cortex-m4 -mthumb -x assembler-with-cpp -c $_CPPDEFFLAGS $_CPPINCFLAGS'
as_flags = '--cpu=Cortex-M4.fp --gnu --split_sections --apcs=interwork --restrict --no_rtti'

################################################################################
# Setup scons environment
# TODO: the part below _should_ be common to all GCC-based compilers, so it might be
# moved to a common place. Let the target definition export just the variables above
# and the rest will happen automatically.
link_script = os.path.join(os.getcwd(), link_script)
env = Environment(
    # C/C++ compiler
    CC = 'armcc', CXX = 'armcc', CCFLAGS = c_flags, CXXFLAGS = cxx_flags, CPPDEFINES = macros,
    # Assembler
    AS = 'armcc', ASFLAGS = as_flags,
    # Archiver (default flags are fine)
    AR = 'armar',
    # Linker
    LINK = 'armlink', LINKFLAGS = ' --scatter ' + link_script,
    # Misc
    ENV = os.environ,
    OBJSUFFIX = '.o',
    PROGSUFFIX = '.elf'
)

# This needs to be added manually, so that it doesn't 'bleed' into the c++ command line
# (which would then throw a warning)
env['CCCOM'] = env['CCCOM'] + ' --c99'

# Let the rest of the world know what kind of compiler we're using
env['compiler_id'] = 'ARMCC'

# [DEBUG] Uncomment the next 3 lines to print all the values in the environment
# [DEBUG] Uncomment the 4th line to exit after printing
# [DEBUG] See http://scons.org/doc/production/HTML/scons-user.html#app-variables for a description of construction variables
#dict = env.Dictionary()
#for k in dict:
#    print k, "=", dict[k]
#Exit(1)

# Builder to get .bin from .elf
post_link = Builder(action = 'arm-none-eabi-objcopy -O binary $SOURCE ${SOURCE}.bin')
env.Append(BUILDERS = {"Bin": post_link})

# We export this so we can add an explicit dependency of the executable on the link script,
# so the executable gets automatically re-linked if the linker command script is changed
# (this needs to happen in the main SConstruct, since the executable target is not yet defined)
Export('env link_script macros')
