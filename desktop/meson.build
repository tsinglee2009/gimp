sizes = [
  '16',
  '22',
  '24',
  '32',
  '48',
  '64',
  '256',
]

foreach size : sizes
  sizedir = size+'x'+size
  install_data(sizedir / 'gimp.png',
    install_dir: get_option('datadir') / 'icons' / 'hicolor' / sizedir / 'apps'
  )
endforeach
install_data('scalable/gimp.svg',
  install_dir: get_option('datadir') / 'icons' / 'hicolor' / 'scalable' / 'apps'
)

desktopfilename = 'gimp.desktop'

desktop_conf = configuration_data()
desktop_conf.set('GIMP_COMMAND',      gimp_command)
desktop_conf.set('GIMP_VERSION',      gimp_version)
desktop_conf.set('GIMP_APP_VERSION',  gimp_app_version)
desktop_conf.set('MIME_TYPES',        ';'.join(MIMEtypes))
desktop_conf.set('BUG_REPORT_URL',    bug_report_url)

desktopfilein = configure_file(
  input : desktopfilename+'.in.in',
  output: desktopfilename+'.in',
  configuration: desktop_conf,
)

desktopfile = custom_target(desktopfilename,
  input : [ desktopfilein, ],
  output: [ desktopfilename, ],
  command: [
    intltool_merge,
    po_dir,
    '@INPUT@',
    '@OUTPUT@',
    '--desktop-style',
    '--utf8',
    '--cache=' + '@OUTDIR@' / 'intltool-merge-cache',
  ],
  install: true,
  install_dir: get_option('datadir') / 'applications',
)

appdatafilename = 'org.gimp.GIMP.appdata.xml'
appdatafilein = configure_file(
  input : appdatafilename+'.in.in',
  output: appdatafilename+'.in',
  configuration: desktop_conf,
)
appdatafile = custom_target(appdatafilename,
  input : [ appdatafilein, ],
  output: [ appdatafilename, ],
  command: [
    intltool_merge,
    po_dir,
    '@INPUT@',
    '@OUTPUT@',
    '--xml-style',
    '--utf8',
    '--cache=' + '@OUTDIR@' / 'intltool-merge-cache',
  ],
  install: true,
  install_dir: get_option('datadir') / 'metainfo',
)

if desktop_validate.found()
  test('validate-desktop',
    desktop_validate,
    args: [ desktopfile ],
    suite: 'desktop',
  )
endif

if appstreamcli.found()
  test('appdata_file',
    appstreamcli,
    args: [ 'validate', appdatafile ],
    env: [
      'GIMP_TESTING_ABS_TOP_BUILDDIR=' + meson.build_root(),
    ],
    suite: 'desktop',
  )
endif
