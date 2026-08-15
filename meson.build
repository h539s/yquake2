project('yquake2', 'c',
  version : '8.40',
  default_options : ['warning_level=2', 'c_std=gnu11'])

cc = meson.get_compiler('c')

# Dependencies
m_dep = cc.find_library('m', required : false)
dl_dep = cc.find_library('dl', required : false)
thread_dep = dependency('threads')
gl_dep = dependency('gl', required: false)

# Options

if get_option('sdl3')
  sdl3_dep = dependency('sdl3', required : true)
  sdl_dep = sdl3_dep
else
  sdl3_dep = dependency('sdl3', required : false)
  sdl2_dep = dependency('sdl2', required : true)
  sdl_dep = sdl2_dep
endif


openal_dep = dependency('openal', required : get_option('openal'))
curl_dep = dependency('libcurl', required : get_option('curl'))

system_minizip = get_option('system_minizip')
if system_minizip
  minizip_dep = dependency('minizip')
endif

# Configuration
conf_data = configuration_data()

host_os = host_machine.system()
if host_os == 'windows'
  conf_data.set_quoted('YQ2OSTYPE', 'Windows')
elif host_os == 'linux'
  conf_data.set_quoted('YQ2OSTYPE', 'Linux')
elif host_os == 'darwin'
  conf_data.set_quoted('YQ2OSTYPE', 'macOS')
elif host_os == 'freebsd'
  conf_data.set_quoted('YQ2OSTYPE', 'FreeBSD')
elif host_os == 'netbsd'
  conf_data.set_quoted('YQ2OSTYPE', 'NetBSD')
elif host_os == 'openbsd'
  conf_data.set_quoted('YQ2OSTYPE', 'OpenBSD')
else
  conf_data.set_quoted('YQ2OSTYPE', 'Unknown')
endif

add_project_arguments('-DYQ2OSTYPE="' + conf_data.get('YQ2OSTYPE').strip('"') + '"', language : 'c')

host_cpu = host_machine.cpu()
if host_cpu == 'x86_64' or host_cpu == 'amd64'
  add_project_arguments('-DYQ2ARCH="x86_64"', language: 'c')
elif host_cpu == 'i386' or host_cpu == 'i686' or host_cpu == 'x86'
  add_project_arguments('-DYQ2ARCH="i386"', language: 'c')
elif host_cpu == 'arm' or host_cpu == 'armv7' or host_cpu == 'armv8' or host_cpu == 'armhf' or host_cpu == 'armel'
  add_project_arguments('-DYQ2ARCH="arm"', language: 'c')
elif host_cpu == 'aarch64' or host_cpu == 'arm64'
  add_project_arguments('-DYQ2ARCH="aarch64"', language: 'c')
else
  add_project_arguments('-DYQ2ARCH="' + host_cpu + '"', language: 'c')
endif


# Source files

# Replace variables with arrays of strings directly


game_src = files('src/common/shared/flash.c', 'src/common/shared/rand.c', 'src/common/shared/shared.c', 'src/game/g_ai.c', 'src/game/g_chase.c', 'src/game/g_cmds.c', 'src/game/g_combat.c', 'src/game/g_func.c', 'src/game/g_items.c', 'src/game/g_main.c', 'src/game/g_misc.c', 'src/game/g_monster.c', 'src/game/g_phys.c', 'src/game/g_spawn.c', 'src/game/g_svcmds.c', 'src/game/g_target.c', 'src/game/g_trigger.c', 'src/game/g_turret.c', 'src/game/g_utils.c', 'src/game/g_weapon.c', 'src/game/monster/berserker/berserker.c', 'src/game/monster/boss2/boss2.c', 'src/game/monster/boss3/boss3.c', 'src/game/monster/boss3/boss31.c', 'src/game/monster/boss3/boss32.c', 'src/game/monster/brain/brain.c', 'src/game/monster/chick/chick.c', 'src/game/monster/flipper/flipper.c', 'src/game/monster/float/float.c', 'src/game/monster/flyer/flyer.c', 'src/game/monster/gladiator/gladiator.c', 'src/game/monster/gunner/gunner.c', 'src/game/monster/hover/hover.c', 'src/game/monster/infantry/infantry.c', 'src/game/monster/insane/insane.c', 'src/game/monster/medic/medic.c', 'src/game/monster/misc/move.c', 'src/game/monster/mutant/mutant.c', 'src/game/monster/parasite/parasite.c', 'src/game/monster/soldier/soldier.c', 'src/game/monster/supertank/supertank.c', 'src/game/monster/tank/tank.c', 'src/game/player/client.c', 'src/game/player/hud.c', 'src/game/player/trail.c', 'src/game/player/view.c', 'src/game/player/weapon.c', 'src/game/savegame/savegame.c')

client_src = files('src/backends/generic/misc.c', 'src/client/cl_cin.c', 'src/client/cl_image.c', 'src/client/cl_console.c', 'src/client/cl_download.c', 'src/client/cl_effects.c', 'src/client/cl_entities.c', 'src/client/cl_input.c', 'src/client/cl_inventory.c', 'src/client/cl_keyboard.c', 'src/client/cl_lights.c', 'src/client/cl_main.c', 'src/client/cl_network.c', 'src/client/cl_parse.c', 'src/client/cl_particles.c', 'src/client/cl_prediction.c', 'src/client/cl_screen.c', 'src/client/cl_tempentities.c', 'src/client/cl_view.c', 'src/client/input/gyro.c', 'src/client/menu/menu.c', 'src/client/menu/qmenu.c', 'src/client/menu/videomenu.c', 'src/client/sound/ogg.c', 'src/client/sound/openal.c', 'src/client/sound/qal.c', 'src/client/sound/sdl.c', 'src/client/sound/sound.c', 'src/client/sound/wave.c', 'src/client/vid/vid.c', 'src/common/argproc.c', 'src/common/clientserver.c', 'src/common/collision.c', 'src/common/crc.c', 'src/common/cmdparser.c', 'src/common/cvar.c', 'src/common/filesystem.c', 'src/common/glob.c', 'src/common/md4.c', 'src/common/movemsg.c', 'src/common/frame.c', 'src/common/netchan.c', 'src/common/pmove.c', 'src/common/szone.c', 'src/common/zone.c', 'src/common/shared/flash.c', 'src/common/shared/rand.c', 'src/common/shared/shared.c', 'src/server/sv_cmd.c', 'src/server/sv_conless.c', 'src/server/sv_entities.c', 'src/server/sv_game.c', 'src/server/sv_init.c', 'src/server/sv_main.c', 'src/server/sv_save.c', 'src/server/sv_send.c', 'src/server/sv_user.c', 'src/server/sv_world.c')
if curl_dep.found()
  client_src += files('src/client/curl/download.c', 'src/client/curl/qcurl.c')
endif
if not system_minizip
  client_src += files('src/common/unzip/ioapi.c', 'src/common/unzip/unzip.c', 'src/common/unzip/miniz/miniz.c', 'src/common/unzip/miniz/miniz_tdef.c', 'src/common/unzip/miniz/miniz_tinfl.c')
endif
if sdl3_dep.found()
  client_src += files('src/client/input/sdl3.c', 'src/client/vid/glimp_sdl3.c')
else
  client_src += files('src/client/input/sdl2.c', 'src/client/vid/glimp_sdl2.c')
endif
if host_os == 'windows'
  client_src += files('src/backends/windows/main.c', 'src/backends/windows/network.c', 'src/backends/windows/system.c', 'src/backends/windows/shared/hunk.c')
else
  client_src += files('src/backends/unix/main.c', 'src/backends/unix/network.c', 'src/backends/unix/signalhandler.c', 'src/backends/unix/system.c', 'src/backends/unix/shared/hunk.c')
endif

server_src = files('src/backends/generic/misc.c', 'src/common/argproc.c', 'src/common/clientserver.c', 'src/common/collision.c', 'src/common/crc.c', 'src/common/cmdparser.c', 'src/common/cvar.c', 'src/common/filesystem.c', 'src/common/glob.c', 'src/common/md4.c', 'src/common/frame.c', 'src/common/movemsg.c', 'src/common/netchan.c', 'src/common/pmove.c', 'src/common/szone.c', 'src/common/zone.c', 'src/common/shared/rand.c', 'src/common/shared/shared.c', 'src/server/sv_cmd.c', 'src/server/sv_conless.c', 'src/server/sv_entities.c', 'src/server/sv_game.c', 'src/server/sv_init.c', 'src/server/sv_main.c', 'src/server/sv_save.c', 'src/server/sv_send.c', 'src/server/sv_user.c', 'src/server/sv_world.c')
if not system_minizip
  server_src += files('src/common/unzip/ioapi.c', 'src/common/unzip/unzip.c', 'src/common/unzip/miniz/miniz.c', 'src/common/unzip/miniz/miniz_tdef.c', 'src/common/unzip/miniz/miniz_tinfl.c')
endif
if host_os == 'windows'
  server_src += files('src/backends/windows/main.c', 'src/backends/windows/network.c', 'src/backends/windows/system.c', 'src/backends/windows/shared/hunk.c')
else
  server_src += files('src/backends/unix/main.c', 'src/backends/unix/network.c', 'src/backends/unix/signalhandler.c', 'src/backends/unix/system.c', 'src/backends/unix/shared/hunk.c')
endif

ref_gl1_src = files('src/client/refresh/gl1/qgl.c', 'src/client/refresh/gl1/gl1_draw.c', 'src/client/refresh/gl1/gl1_image.c', 'src/client/refresh/gl1/gl1_light.c', 'src/client/refresh/gl1/gl1_lightmap.c', 'src/client/refresh/gl1/gl1_main.c', 'src/client/refresh/gl1/gl1_mesh.c', 'src/client/refresh/gl1/gl1_misc.c', 'src/client/refresh/gl1/gl1_model.c', 'src/client/refresh/gl1/gl1_scrap.c', 'src/client/refresh/gl1/gl1_surf.c', 'src/client/refresh/gl1/gl1_warp.c', 'src/client/refresh/gl1/gl1_sdl.c', 'src/client/refresh/gl1/gl1_buffer.c', 'src/client/refresh/files/common.c', 'src/client/refresh/files/surf.c', 'src/client/refresh/files/models.c', 'src/client/refresh/files/pcx.c', 'src/client/refresh/files/stb.c', 'src/client/refresh/files/wal.c', 'src/client/refresh/files/pvs.c', 'src/common/shared/shared.c', 'src/common/md4.c')
if host_os == 'windows'
  ref_gl1_src += files('src/backends/windows/shared/hunk.c')
else
  ref_gl1_src += files('src/backends/unix/shared/hunk.c')
endif
ref_gl1_glad_src = files('src/client/refresh/gl1/glad-gles1/src/glad.c')

ref_gl3_src = files('src/client/refresh/gl3/gl3_draw.c', 'src/client/refresh/gl3/gl3_image.c', 'src/client/refresh/gl3/gl3_light.c', 'src/client/refresh/gl3/gl3_lightmap.c', 'src/client/refresh/gl3/gl3_main.c', 'src/client/refresh/gl3/gl3_mesh.c', 'src/client/refresh/gl3/gl3_misc.c', 'src/client/refresh/gl3/gl3_model.c', 'src/client/refresh/gl3/gl3_sdl.c', 'src/client/refresh/gl3/gl3_surf.c', 'src/client/refresh/gl3/gl3_warp.c', 'src/client/refresh/gl3/gl3_shaders.c', 'src/client/refresh/files/common.c', 'src/client/refresh/files/surf.c', 'src/client/refresh/files/models.c', 'src/client/refresh/files/pcx.c', 'src/client/refresh/files/stb.c', 'src/client/refresh/files/wal.c', 'src/client/refresh/files/pvs.c', 'src/common/shared/shared.c', 'src/common/md4.c')
if host_os == 'windows'
  ref_gl3_src += files('src/backends/windows/shared/hunk.c')
else
  ref_gl3_src += files('src/backends/unix/shared/hunk.c')
endif
ref_gl3_glad_src = files('src/client/refresh/gl3/glad/src/glad.c')
ref_gles3_glad_src = files('src/client/refresh/gl3/glad-gles3/src/glad.c')

ref_soft_src = files('src/client/refresh/soft/sw_aclip.c', 'src/client/refresh/soft/sw_alias.c', 'src/client/refresh/soft/sw_bsp.c', 'src/client/refresh/soft/sw_draw.c', 'src/client/refresh/soft/sw_edge.c', 'src/client/refresh/soft/sw_image.c', 'src/client/refresh/soft/sw_light.c', 'src/client/refresh/soft/sw_main.c', 'src/client/refresh/soft/sw_misc.c', 'src/client/refresh/soft/sw_model.c', 'src/client/refresh/soft/sw_part.c', 'src/client/refresh/soft/sw_poly.c', 'src/client/refresh/soft/sw_polyset.c', 'src/client/refresh/soft/sw_rast.c', 'src/client/refresh/soft/sw_scan.c', 'src/client/refresh/soft/sw_sprite.c', 'src/client/refresh/soft/sw_surf.c', 'src/client/refresh/files/surf.c', 'src/client/refresh/files/common.c', 'src/client/refresh/files/models.c', 'src/client/refresh/files/pcx.c', 'src/client/refresh/files/stb.c', 'src/client/refresh/files/wal.c', 'src/client/refresh/files/pvs.c', 'src/common/shared/shared.c', 'src/common/md4.c')
if host_os == 'windows'
  ref_soft_src += files('src/backends/windows/shared/hunk.c')
else
  ref_soft_src += files('src/backends/unix/shared/hunk.c')
endif


if host_os == 'windows'
  win_deps = [cc.find_library('ws2_32'), cc.find_library('winmm')]
else
  win_deps = []
endif

windows = import('windows')
win_res = []
if host_os == 'windows'
  win_res = windows.compile_resources('src/backends/windows/icon.rc')
endif

executable_args = []
if host_os == 'windows'
  executable_args += ['-mwindows']
endif

# Common args
c_args = ['-Wall', '-fno-strict-aliasing', '-fwrapv']
# MSVC might need different flags
if cc.get_id() == 'msvc'
  c_args = ['/wd4244', '/wd4305', '/wd4018', '/wd4996']
else
  c_args += ['-fvisibility=hidden']
endif


if get_option('xdg')
  c_args += ['-DUSE_XDG=1']
endif

if sdl3_dep.found()
  c_args += ['-DUSE_SDL3']
endif

if get_option('systemwide')
  c_args += ['-DSYSTEMWIDE']
  if get_option('systemdir') != ''
    c_args += ['-DSYSTEMDIR="' + get_option('systemdir') + '"']
  endif
endif

if curl_dep.found()
  c_args += ['-DUSE_CURL']
endif


if host_os == 'windows'
  c_args += ['-DDEFAULT_OPENAL_DRIVER="openal32.dll"']
elif host_os == 'darwin'
  c_args += ['-DDEFAULT_OPENAL_DRIVER="libopenal.dylib"']
else
  c_args += ['-DDEFAULT_OPENAL_DRIVER="libopenal.so.1"']
endif

if openal_dep.found()
  c_args += ['-DUSE_OPENAL']
endif

# Build client

client_target = executable(host_os == 'windows' ? 'yquake2' : 'quake2',
    client_src, win_res,
    dependencies : [sdl_dep, openal_dep, curl_dep, dl_dep, m_dep, thread_dep] + win_deps + (cc.has_header('execinfo.h') and (host_os in ['freebsd', 'netbsd', 'openbsd']) ? [cc.find_library('execinfo')] : []) + (system_minizip ? [minizip_dep] : []),
    c_args : c_args,
    link_args : executable_args,
    install : true
  )

# Build server

server_target = executable('q2ded',
    server_src, win_res,
    dependencies : [dl_dep, m_dep, thread_dep] + win_deps + (cc.has_header('execinfo.h') and (host_os in ['freebsd', 'netbsd', 'openbsd']) ? [cc.find_library('execinfo')] : []) + (system_minizip ? [minizip_dep] : []),
    c_args : c_args + ['-DDEDICATED_ONLY'],
    link_args : executable_args,
    install : true
  )

# Shared modules (Renderers, Game)

# baseq2

game_target = shared_module('game',
    game_src,
    name_prefix : '',
    dependencies : [m_dep],
    c_args : c_args,
    install : true,
    install_dir : 'baseq2'
  )


ref_gl1_target = shared_module('ref_gl1',
    ref_gl1_src,
    name_prefix : '',
    dependencies : [m_dep, sdl_dep, gl_dep],
    c_args : c_args,
    install : true
  )


ref_gl3_target = shared_module('ref_gl3',
    ref_gl3_src + ref_gl3_glad_src,
    include_directories : include_directories('src/client/refresh/gl3/glad/include'),
    name_prefix : '',
    dependencies : [m_dep, sdl_dep, gl_dep],
    c_args : c_args,
    install : true
  )


ref_gles1_target = shared_module('ref_gles1',
    ref_gl1_src + ref_gl1_glad_src,
    include_directories : include_directories('src/client/refresh/gl1/glad-gles1/include'),
    name_prefix : '',
    dependencies : [m_dep, sdl_dep, gl_dep],
    c_args : c_args + ['-DYQ2_GL1_GLES=1'],
    install : true
  )


ref_gles3_target = shared_module('ref_gles3',
    ref_gl3_src + ref_gles3_glad_src,
    include_directories : include_directories('src/client/refresh/gl3/glad-gles3/include'),
    name_prefix : '',
    dependencies : [m_dep, sdl_dep, gl_dep],
    c_args : c_args + ['-DYQ2_GL3_GLES3=1', '-DYQ2_GL3_GLES=1'],
    install : true
  )


ref_soft_target = shared_module('ref_soft',
    ref_soft_src,
    name_prefix : '',
    dependencies : [m_dep, sdl_dep, gl_dep],
    c_args : c_args,
    install : true
  )







alias_target('client', client_target)
alias_target('server', server_target)
