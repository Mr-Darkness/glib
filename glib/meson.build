configure_file(input : 'glibconfig.h.in', output : 'glibconfig.h',
  install : true,
  install_dir : join_paths(get_option('libdir'), 'glib-2.0/include'),
  configuration : glibconfig_conf)

subdir('libcharset')
if not pcre.found()
  subdir('pcre')
endif

glib_headers = [
  'glib.h',
  'glib-unix.h',
  'glib-object.h',
]
install_headers(glib_headers, subdir : 'glib-2.0')

glib_deprecated_headers = [
  'deprecated/gallocator.h',
  'deprecated/gcache.h',
  'deprecated/gcompletion.h',
  'deprecated/gmain.h',
  'deprecated/grel.h',
  'deprecated/gthread.h',
]
install_headers(glib_deprecated_headers, subdir : 'glib-2.0/glib/deprecated')

glib_sub_headers = [
  'glib-autocleanups.h',
  'galloca.h',
  'garray.h',
  'gasyncqueue.h',
  'gatomic.h',
  'gbacktrace.h',
  'gbase64.h',
  'gbitlock.h',
  'gbookmarkfile.h',
  'gbytes.h',
  'gcharset.h',
  'gchecksum.h',
  'gconvert.h',
  'gdataset.h',
  'gdate.h',
  'gdatetime.h',
  'gdir.h',
  'genviron.h',
  'gerror.h',
  'gfileutils.h',
  'ggettext.h',
  'ghash.h',
  'ghmac.h',
  'ghook.h',
  'ghostutils.h',
  'gi18n.h',
  'gi18n-lib.h',
  'giochannel.h',
  'gkeyfile.h',
  'glist.h',
  'gmacros.h',
  'gmain.h',
  'gmappedfile.h',
  'gmarkup.h',
  'gmem.h',
  'gmessages.h',
  'gnode.h',
  'goption.h',
  'gpattern.h',
  'gpoll.h',
  'gprimes.h',
  'gqsort.h',
  'gquark.h',
  'gqueue.h',
  'grand.h',
  'gregex.h',
  'gscanner.h',
  'gsequence.h',
  'gshell.h',
  'gslice.h',
  'gslist.h',
  'gspawn.h',
  'gstdio.h',
  'gstrfuncs.h',
  'gtestutils.h',
  'gstring.h',
  'gstringchunk.h',
  'gthread.h',
  'gthreadpool.h',
  'gtimer.h',
  'gtimezone.h',
  'gtrashstack.h',
  'gtree.h',
  'gtypes.h',
  'gunicode.h',
  'gurifuncs.h',
  'gutils.h',
  'gvarianttype.h',
  'gvariant.h',
  'gversion.h',
  'gversionmacros.h',
  'gwin32.h',
  'gprintf.h',
]
install_headers(glib_sub_headers, subdir : 'glib-2.0/glib')

deprecated_sources = [
  'deprecated/gallocator.c',
  'deprecated/gcache.c',
  'deprecated/gcompletion.c',
  'deprecated/grel.c',
  'deprecated/gthread-deprecated.c'
]

glib_sources = [
  'garray.c',
  'gasyncqueue.c',
  'gatomic.c',
  'gbacktrace.c',
  'gbase64.c',
  'gbitlock.c',
  'gbookmarkfile.c',
  'gbytes.c',
  'gcharset.c',
  'gchecksum.c',
  'gconvert.c',
  'gdataset.c',
  'gdate.c',
  'gdatetime.c',
  'gdir.c',
  'genviron.c',
  'gerror.c',
  'gfileutils.c',
  'ggettext.c',
  'ghash.c',
  'ghmac.c',
  'ghook.c',
  'ghostutils.c',
  'giochannel.c',
  'gkeyfile.c',
  'glib-init.c',
  'glib-private.c',
  'glist.c',
  'gmain.c',
  'gmappedfile.c',
  'gmarkup.c',
  'gmem.c',
  'gmessages.c',
  'gnode.c',
  'goption.c',
  'gpattern.c',
  'gpoll.c',
  'gprimes.c',
  'gqsort.c',
  'gquark.c',
  'gqueue.c',
  'grand.c',
  'gregex.c',
  'gscanner.c',
  'gsequence.c',
  'gshell.c',
  'gslice.c',
  'gslist.c',
  'gstdio.c',
  'gstrfuncs.c',
  'gstring.c',
  'gstringchunk.c',
  'gtestutils.c',
  'gthread.c',
  'gthreadpool.c',
  'gtimer.c',
  'gtimezone.c',
  'gtranslit.c',
  'gtrashstack.c',
  'gtree.c',
  'guniprop.c',
  'gutf8.c',
  'gunibreak.c',
  'gunicollate.c',
  'gunidecomp.c',
  'gurifuncs.c',
  'gutils.c',
  'gvariant.c',
  'gvariant-core.c',
  'gvariant-parser.c',
  'gvariant-serialiser.c',
  'gvarianttypeinfo.c',
  'gvarianttype.c',
  'gversion.c',
  'gwakeup.c',
  'gprintf.c',
]

if host_system == 'windows'
  thread_src = ['gthread-win32.c']
  plat_src = ['gwin32.c', 'gspawn-win32.c', 'giowin32.c']
  platform_deps = [winsock2, cc.find_library('winmm')]
else
  thread_src = ['gthread-posix.c']
  plat_src = ['glib-unix.c', 'gspawn.c', 'giounix.c']
  platform_deps = []
endif

libglib = shared_library('glib-2.0',
  sources : [deprecated_sources, glib_sources, thread_src, plat_src],
  version : library_version,
  soversion : soversion,
  install : true,
  # intl.lib is not compatible with SAFESEH
  link_args : noseh_link_args,
  include_directories : configinc,
  link_with : charset_lib,
  dependencies : [pcre, thread_dep, libintl, librt] + libiconv + platform_deps,
  c_args : ['-DG_LOG_DOMAIN="GLib"', '-DGLIB_COMPILATION', '-DPCRE_STATIC']
)

libglib_dep = declare_dependency(link_with : libglib,
  # We sadly need to export configinc here because everyone includes <glib/*.h>
  include_directories : [configinc, glibinc])

# On Windows, glib needs a spawn helper for g_spawn* API
if host_system == 'windows'
  if host_machine.cpu_family() == 'x86'
    executable('gspawn-win32-helper', 'gspawn-win32-helper.c',
      install : true,
      gui_app : true,
      c_args : ['-DHAVE_CONFIG_H=1'],
      include_directories : configinc,
      dependencies : [libglib_dep])
    executable('gspawn-win32-helper-console', 'gspawn-win32-helper.c',
      install : true,
      c_args : ['-DHAVE_CONFIG_H=1', '-DHELPER_CONSOLE'],
      include_directories : configinc,
      dependencies : [libglib_dep])
  else
    executable('gspawn-win64-helper', 'gspawn-win32-helper.c',
      install : true,
      gui_app : true,
      c_args : ['-DHAVE_CONFIG_H=1'],
      include_directories : configinc,
      dependencies : [libglib_dep])
    executable('gspawn-win64-helper-console', 'gspawn-win32-helper.c',
      install : true,
      c_args : ['-DHAVE_CONFIG_H=1', '-DHELPER_CONSOLE'],
      include_directories : configinc,
      dependencies : [libglib_dep])
  endif
else
  executable('gtester', 'gtester.c',
    install : true,
    c_args : ['-DHAVE_CONFIG_H=1'],
    include_directories : configinc,
    dependencies : [libglib_dep])
endif

install_data('gtester-report', install_dir : get_option('bindir'))
install_data('glib.py', install_dir : join_paths(glib_pkgdatadir, 'gdb'))

gdb_conf = configuration_data()
gdb_conf.set('datadir', glib_datadir)
configure_file(
  input: 'libglib-gdb.py.in',
  output: 'libglib-2.0.so.@0@-gdb.py'.format(library_version),
  configuration: gdb_conf,
  install: true,
  install_dir: join_paths(get_option('datadir'), 'gdb/auto-load' + glib_libdir)
)

# gtester doesn't work on native windows
if cc.get_id() != 'msvc'
  subdir('tests')
endif