#!/usr/bin/env groovy

@Library('KopyrinLib')
import org.barabuga.build
var b = new org.barabuga.build.Build()

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

timestamps {
	stage('build') {
		parallel(
			"build_win": {
				if (_buildWin)
					b.buildWin("windows", "build_win")
			},
			"build_mac": {
				if (_buildMac)
					b.buildUnix("mac", "build_mac")
			},
			"build_linux": {
				if (_buildUnix)
					b.buildUnix("linux", "build_unix")
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