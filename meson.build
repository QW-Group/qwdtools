project('qwdtools', 'c')

qwdtools_sources = [
	'src/bothtools.c',
	'src/dem_parse.c',
	'src/dem_send.c',
	'src/ini.c',
	'src/init.c',
	'src/main.c',
	'src/marge.c',
	'src/qwz.c',
	'src/sync.c',
	'src/tools.c',
]

c_args = ['-DSERVERONLY', '-DUSE_PR2']
link_args = []

deps = [
	dependency('threads')
]


if target_machine.system() == 'windows'
	deps += [
		meson.get_compiler('c').find_library('ws2_32'),
		meson.get_compiler('c').find_library('winmm'),
	]
	if meson.get_compiler('c').get_id() == 'gcc'
		link_args += '-static-libgcc'
	endif
else
	deps += [
		meson.get_compiler('c').find_library('m'),
		meson.get_compiler('c').find_library('dl'),
	]
endif

if target_machine.endian() == 'little'
	c_args += '-D__LITTLE_ENDIAN__Q__'
elif target_machine.endian() == 'big'
	c_args += '-D__BIG_ENDIAN__Q__'
else
	c_args += '-D__PDP_ENDIAN__Q__'
endif

if target_machine.cpu_family() == 'x86' and target_machine.system() != 'darwin'
	asm_c_args = ['-x', 'assembler-with-cpp', '-Did386']
	if target_machine.system() != 'windows'
		asm_c_args += '-DELF'
	endif
	deps += declare_dependency(
		link_with : static_library('asm',
			'src/bothtoolsa.s',
			c_args : asm_c_args
		)
	)
endif

executable('qwdtools', qwdtools_sources,
	dependencies : deps,
	c_args : c_args,
	link_args : link_args
)
