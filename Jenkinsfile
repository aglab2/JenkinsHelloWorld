#!/usr/bin/env groovy

Boolean _buildMac = true
try {
	_buildMac = buildMac.toBoolean()
} catch (MissingPropertyException ignore) {
	_buildMac = true
}

Boolean _buildWin = true
try {
	_buildWin = buildWin.toBoolean()
} catch (MissingPropertyException ignore) {
	_buildWin = true
}

Boolean _buildUnix = true
try {
	_buildUnix = buildUnix.toBoolean()
} catch (MissingPropertyException ignore) {
	_buildUnix = true
}

def buildUnix(label, stashName) {
	node(label) {
		checkout scm
		sh """#!/bin/sh
			gcc hello.c -o hello_${label}.out
		"""
		stash name: stashName, includes: "*.out"
		deleteDir()
	}
}

def buildWin(label, stashName) {
	node(label) {
		checkout scm
		String vsvars_bat = 'Microsoft Visual Studio 12.0\\VC\\vcvarsall.bat'
		bat """
			call "%ProgramFiles(X86)%\\${vsvars_bat}" x86
			cl.exe hello.c
		"""
		stash name: stashName, includes: "*.exe"
		deleteDir()
	}
}

timestamps {
	stage('build') {
		parallel(
			"build_win": {
				if (_buildWin)
					buildWin("windows", "build_win")
			},
			"build_mac": {
				if (_buildMac)
					buildUnix("mac", "build_mac")
			},
			"build_linux": {
				if (_buildUnix)
					buildUnix("linux", "build_unix")
			}
		)
	}
	
	stage('store') {
		node("master") {
			if (_buildMac)
				unstash "build_mac"
				
			if (_buildUnix)
				unstash "build_unix"
			
			if (_buildWin)
				unstash "build_win"

			archiveArtifacts artifacts: '*'
			deleteDir()
		}
	}
}