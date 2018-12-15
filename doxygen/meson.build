doxygen = find_program('doxygen', required: get_option('docs'))

if not doxygen.found()
  subdir_done()
endif

top_srcdir = meson.source_root()
builddir = meson.current_build_dir()

doxy_cdata = configuration_data()

doxy_cdata.set('MAINPAGE_H', join_paths(top_srcdir, 'doxygen', 'mainpage.h'))
doxy_cdata.set('SRCDIR', join_paths(top_srcdir, 'src'))
doxy_cdata.set('INCLUDEDIR', join_paths(top_srcdir, 'include', 'tinyalsa'))
doxy_cdata.set('EXAMPLE_DIR', join_paths(top_srcdir, 'examples'))
doxy_cdata.set('HTML_OUTPUT_DIR', join_paths(builddir, 'html'))
doxy_cdata.set('MAN_OUTPUT_DIR', join_paths(builddir))
doxy_cdata.set('MAN_OUTPUT_SUBDIR', './') # we want no subdir

# We need two separate runs for html and man so we can install the output
# in different target directories (man pages go into mandir, html into docdir)

if not get_option('docs-html').disabled()
  doxy_cdata_html = doxy_cdata
  doxy_cdata_html.set('GENERATE_HTML', 'YES')
  doxy_cdata_html.set('GENERATE_MAN', 'NO')

  doxyfile_html = configure_file(input: 'Doxyfile.in',
    output: 'Doxyfile-html',
    configuration: doxy_cdata_html,
    install: false)

  docdir = join_paths(get_option('datadir'), 'doc', 'tinyalsa')
  custom_target('tinyalsa-docs-html',
    input: doxyfile_html,
    output: 'html',
    command: [doxygen, '@INPUT@'],
    install: true,
    install_dir: docdir)
endif

if not get_option('docs-man').disabled()
  doxy_cdata_man = doxy_cdata
  doxy_cdata_man.set('GENERATE_MAN', 'YES')
  doxy_cdata_man.set('GENERATE_HTML', 'NO')

  doxyfile_man = configure_file(input: 'Doxyfile.in',
    output: 'Doxyfile-man',
    configuration: doxy_cdata_man,
    install: false)

  man3dir = join_paths(get_option('mandir'), 'man3')
  custom_target('tinyalsa-docs-man',
    input: doxyfile_man,
    output: ['libtinyalsa-pcm.3', 'libtinyalsa-mixer.3'],
    command: [doxygen, '@INPUT@'],
    install: true,
    install_dir: man3dir)
endif
