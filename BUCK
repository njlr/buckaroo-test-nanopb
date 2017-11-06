include_defs('//BUCKAROO_DEPS')

import os
import hashlib

genrule(
  name = 'simple-protobuf',
  out = 'out',
  srcs = [
    'simple.proto',
  ],
  cmd = 'mkdir $OUT && protoc --cpp_out=$OUT simple.proto',
)

def extract(rule, path):
  filename, extension = os.path.splitext(path)
  name = filename + '-' + hashlib.sha256(rule + path).hexdigest()[:8] + extension
  genrule(
    name = name,
    out = name,
    cmd = 'cp $(location ' + rule + ')/' + path + ' $OUT',
  )
  return ':' + name

cxx_library(
  name = 'simple',
  header_namespace = '',
  exported_headers = {
    'simple.pb.h': extract(':simple-protobuf', 'simple.pb.h'),
  },
  srcs = [
    extract(':simple-protobuf', 'simple.pb.cc'),
  ],
  deps = BUCKAROO_DEPS, # For the protobuf run-time
)

cxx_test(
  name = 'test1',
  srcs = [
    'test1.cpp',
    ':simple',
  ],
  deps = BUCKAROO_DEPS,
)
