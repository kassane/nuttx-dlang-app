############################################################################
# apps/examples/hello_d/Makefile
#
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.  The
# ASF licenses this file to you under the Apache License, Version 2.0 (the
# "License"); you may not use this file except in compliance with the
# License.  You may obtain a copy of the License at
#
#   http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
# WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.  See the
# License for the specific language governing permissions and limitations
# under the License.
#
############################################################################

include $(APPDIR)/Make.defs

# Hello, World! D Example

MAINSRC = hello_d_main.d

# hello_d built-in application info

DFLAGS += -betterC -vtls -verrors=context -fno-exceptions
# D TLS (thread local storage) when use `static` keyword
# or replace to `__gshared` (C static) and remove this DFLAGS line
DFLAGS += --d-version=NuttX_D_Initialize --emulated-tls
# Verbose: D compiler output
DFLAGS += -vdmd
# importC: need $(CC) to compile C file and -P-I to import C includes
DFLAGS += -P-I$(TOPDIR)/include --d-version=NuttX_ImportC --gcc=$(CC)

PROGNAME  = $(CONFIG_EXAMPLES_HELLO_D_PROGNAME)
PRIORITY  = $(CONFIG_EXAMPLES_HELLO_D_PRIORITY)
STACKSIZE = $(CONFIG_EXAMPLES_HELLO_D_STACKSIZE)
MODULE    = $(CONFIG_EXAMPLES_HELLO_D)

include $(APPDIR)/Application.mk
